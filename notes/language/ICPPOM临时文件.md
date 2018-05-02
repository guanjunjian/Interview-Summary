# 第7章 站在对象模型的尖端

本章包括三个方向的问题：

- 1.模板（template）
- 2.异常处理（exception handling，EH）
- 3.执行期类型识别（runtime type identification，RTTI）

## 7.1 模板

**实例化**：进程将真正的类型和表达式绑定到模板相关形式参数上

三个主要讨论方向：

- 1.**模板的声明**：当你声明一个模版类时会发生什么事情 
- 2.如何实例化**类对象**、**内联非成员对象**、**成员模板函数**：每一个编译单位都会拥有一份实例
- 3.如何实例化**非成员模板函数**、**成员模板函数**、**静态模板类成员**：每一个执行文件中只需要一份实例

### 7.1.1 模板的实例化行为

```c++
template< class Type >
class Point
{
public:
    enum Status { unallocated, normalized };
    
    Point( Type x = 0.0, Type y = 0.0, Type z = 0.0 );
    ~Point();
    
    void* operator new( size_t );
    void operator delete( void*, size_t );

private:
    static Point< Type > *freeList;
    static int chunkSize;
    Type _x, _y, _z;
};
```

- 1.template class声明时，实际程序中什么也没有做
  - 静态数据成员、内嵌enum和enumerators并不可用
  - 以上三种类型的数据只能通过某个实例来存取或操纵

```c++
//错误行为
Point::Status s;
Point::Status::freeList;

//正确行为
Point< float >::Status s;
Point< float >::freeList;
```

- 2.每个静态数据成员对应一种模板实例

```c++
//与Point类的float实例产生关联
Point< float >::freeList;
//与Point类的double实例产生关联
Point< double >::freeList;
```

- 3.指向值为0的模板的指针和引用
  - 指针：什么也没有发生
  - 引用：会实例化一个对应的模板实例

```c++
//什么也没有发生
Point< float > *ptr = 0;

//实例化一个Point的float实例
const Point< float > &ref = 0;
//编译器的行为
Point< float > temporary( float(0) );
const Point< float > &ref = temporary;
```

- 4.模板对象
  - 会产生模板类的实例化

```c++
//Point的非静态成员在此时被实例化，其Type被绑定为float
//但成员函数此时还不实例化，看5.
const Point< float > origin;
```

- 5.成员函数的实例化
  - **是否实例化**：未被使用过的成员函数不应该被实例化，只有在成员函数被使用的时候，C++标准才要求它们被实例化
  - **实例化时间**：
    - 编译时
    - 链接时

### 7.1.2 模板的错误报告

编译时期只会解析词汇错误和解析错误，而模版的类型检查则推迟到模版被实例化的时候进行，这会导致一些非常明显的模版错误不能被及时检测出来：

```c++
template <class type>
class Foo{
	public:
		Foo();
		type val();
		void val(type);
	private:
		type _val;
};

//Foo类中没有bogus_member成员函数，这样的明显的错误，编译时期确并不能发现
template <class type> double Foo<type>::bogus_member(){
	//Foo中也没有dbx数据成员
    return this->dbx;
}
```

编译器会将模版的声明收集成为一系列的“lexical tokens”（词汇标记），解析模版的操作则需要等到真正实例化操作时才开始，会将这些tokens**推送到解析器**， 然后调用类型检验

### 7.1.3 模版中的名称决议法

需要区分“**模板声明作用域**”和“**模板实例化作用域**”

对于一个非成员名字的决议结果，是根据这个名字的使用是否与“**用以实例化该模板的参数类型**”有关而决定的

- 与实例化模板参数类型**无关**：以“**模板声明作用域**”来决定name
- 与实例化模板参数类型有**关**：以“**模板实例化作用域**”来决定name

（//TODO：与自己在VS中测的结果不一致）

```c++
extern double foo(double);

template <class type>
class ScopeRules {
public:
	void invariant() {
		_member = foo(_val);
	}
	type type_dependent() {
		return foo(_member);
	}

private:
	int _val;
	type _member;

};


extern int foo(int);
ScopeRules<int> sr0;
sr0.invariant(); //调用extern double foo(double);
sr0.type_dependent(); //调用extern int foo(int);
```

前提：函数的决议结果只和函数的原型有关，和函数的返回值没有关系

- 1.`invariant()`：因为_val的类型是int，与“**用以实例化该模板的参数类型**”无关，因此调用操作根据“**模板声明作用域**”来决定foo()的调用，此时可选的函数只有1个，因此调用`extern double foo(double);`
- 2.`type_dependent()`：因为_member的类型是Type，与“**用以实例化该模板的参数类型**”有关，因此调用操作根据“**模板实例化作用域**”来决定foo()的调用，此时可选的函数有2个：`extern double foo(double);`和`extern int foo(int);`，由于实例化Type为int，因此调用`extern int foo(int);`

### 7.1.4 成员函数的实例化行为

模版函数实例化的策略目前主要有两种： 

- 1.编译时期策略 
- 2.链接时期策略 

编译器设计者必须解决的三个问题：

- 1.编译器如何找到函数定义 
  - 方法一：包含一个template program text file,像头文件一样
  -  方法二：要求一个文件命名规则，如我们在Point.h中发现的函数声明，其template program text一定要放置于文件Point.c,Point.cpp中 
- 2.编译器如何能够只实例化程序中用到的成员函数 
  - 方法一：忽略这个要求，把实例化类的所有成员函数都产生出来 
  - 方法二：模拟链接操作，检测哪个函数需要，只为它们生产实例 
- 3.编译器如何阻止成员函数在多个.o文件中被实例化？ 
  - 方法一：产生多个实例，然后从链接器中获得支持，最后只留下一个
  - 方法二： 由使用则引导“模拟链接阶段”的实例化策略 ，决定哪些实例才是所需要的

## 7.2 异常处理

