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

### 7.2.1 异常处理快速检阅

C++异常处理由三个主要的词汇组件构成：

- 1.一个**throw**语句，它在程序某处发出一个异常，被抛出的异常类型可以是内建类型，也可以是用户自定义类型

- 2.一个或多个**catch**子句，每一个catch子句都是一个“异常处理”,提供实际的处理程序

- 3.一个**try**区段，区段中有一些列的语句，这些语句可能会引发catch子句起作用

当一个异常抛出的时候，控制权会从函数调用中被释放出来，并寻找一个吻合的catch子句，如果没有吻合者，则调用默认的处理例程terminate()

不断寻找catch子句的过程叫做**栈展开**，栈展开的过程中，将一个个函数从栈中推出，在每一个函数被推出栈之前，会先调用该函数中局部对象的析构函数



异常处理改变了函数在资源管理上的语意，如下面函数含有一块共享内存的locking和unlocking操作，虽然看起来和exception没有什么关系，但在exception之下并不能保证能够正确运行： 

```c++
void mumble(void *arena){
	Point *p = new Point;
	smLock(arena);//对共享区域加锁
	...//如果这里出现异常，程序不能正确运行，因为异常处理并不会去对该共享内存进行解锁操作
	smUnLock(arena);//解锁
	delete p;
}
```

解决方法：添加一个default catch子句，处理正常异常处理机制所不能处理的问题：

```c++
void mumble(void *arena){
	Point *p;
	p = new Point;//new如果出现异常，new自身会有heap上的析构处理，也就不需要delete操作，所以不需要在try block区域内
	try{
		smLock(arena);
		//...
	}
	catch(...){
		//添加共享内存解锁操作
		smUnLock(arena);
		delete p ;
		throw;
	}
	smUnLock(arena);
	delete p;

}
```

建议：将资源需求封装于一个类对象体内，由析构函数来释放资源，如下：

### 7.2.2 对异常处理的支持

当一个异常发生时，编译系统必须完成以下事情：

- 1.检验发生throw操作的函数 
- 2.决定throw操作是否发生在try区段中 
- 3.若是，则将异常类型和每个catch子句进行比较 
- 4.如果比较后吻合，流程控制交给catch子句 
- 5.如果throw的发生并不在try区段中，或没有一个catch子句吻合则 
  - 摧毁所有激活的局部对象 
  - 从堆栈中将目前的函数弹出（unwind）
  - 进入到程序堆栈中的下一个函数中，重复上述2~5

> 7.2.2.1 决定throw是否发生在一个try区段中

包含异常处理的函数可以分为三个区域：

- 1.try区段以外的区域，没有局部变量被激活 
- 2.try区段以外的区域，但有一个以上的局部对象被激活，并且需要析构 
- 3.try区段以内的区域 

编译器通过“**程序计数范围表格**”（program counter-range表格）标示以上三种区域

为了在一个内含try区段的函数中标出某个区域，可以把程序计数（program counter）的起始值和结束值存储在一个表格中

**查找过程**：

当throw操作发生时，目前程序计数器中的值拿来和对应的"范围表格"进行比对，以决定目前作用中的区域是否在一个try区段中，如果是，则找出相关的catch子句，如果这个exception无法被处理，该函数会从函数栈中推出，**程序计数**会被设定为调用端地址，这样的循环再重新开始

> 7.2.2.2 将异常的类型和每一个catch子句的类型做比较

**对于抛出的异常**

每一个抛出来的异常，编译器必须产生一个类型描述器，对异常类型进行编码，如果该异常是一个派生类，则编码中必须要包括所有基类的类型信息。RTTI正是因为支持异常处理而获得的副产品

**对于catch子句**

每一个catch子句也产生一个类型描述器

**对于整个函数**

每一个函数都会产生一个exception表格，它描述与函数相关的各区域、任何必要的善后处理代码、catch子句的位置

**比较过程**

执行期的异常处理将“抛出对象的类型描述器“和”每一个catch子句的类型描述器“进行比较，直到找到一个吻合的、或是直到堆栈已经被完全展开而terminate()被调用

### 7.2.3 当一个实际对象在程序执行时被抛出发生的事情

当一个异常被抛出时，异常对象会被产生出来并通常放置在相同形式的异常数据堆栈中。从throw端传给catch子句的是：**异常对象的地址**、**类型描述器**（或是一个函数指针，该函数会传回与该异常类型有关的类型描述器对象）、可能会有的**异常对象描述器**

考虑如下问题：

有一个catch子语句：

```c++
catch( exPoint p ){
	//do something;
	throw;
}
```

和一个异常对象，类型为exVertex，派生自exPoint

p会发生如下事情：

- 1.由于p是一个对象参数，所以会将抛出的异常exVertex**拷贝构造**给catch子句内部的局部变量 
- 2.由于p是一个对象而不是一个引用，当其内容被拷贝的时候，异常对象的非exPoint部分会被切掉。如果异常对象有虚函数，异常对象的虚函数表指针会指向exPoint的虚函数表，因此以对象为参数的catch语句无法**动态处理**
- 3.并且catch子句中对异常的修改不会作用到原异常对象 

优化：**使用引用**

```c++
catch(exPoint &p ){
	//do something;
	throw;
}
```

另一个问题：**全局异常对象**

```c++
exVertex errVer;

muble()
{
    if( muble_cond )
    {
        errVer.fileName("muble()");
        throw errVer;
    }
}
```

构造并放置于异常堆栈中的是全局变量errVer的复制品，catch子句对于异常对象的改变都是局部性的，不会影响errVer。当一个catch子句评估完毕并且不会再抛出异常后，该复制品被销毁（//TODO：这里不是很理解）

## 7.3 执行期类型识别

