# 第6章 执行期语意学

## 6.1 对象的构造和析构

### 6.1.1 全局对象

```c++
//这是一个全局对象
Matrix identity;

main()
{
    //identity必须在此处被初始化
    Matrix m1 = identity;
    ...
    return 0;
}
```

C++保证，一定会在main()函数中第一次用到identity之前，把identity构造出来，而在main()函数结束之前吧identity摧毁掉

像identity这样的全局变量如果有构造函数和析构函数的话，我们说它需要**静态的初始化操作和内存释放操作**



**munch策略：**

一个可移植但成本颇高的**全局变量静态初始化和内存释放**方法（一个比较过时的方法）

策略的具体描述为：

- 1.为每一个需要静态初始化的文件产生一个_sti()函数，内含必要的构造函数调用操作或构造函数的内联扩展
  - 例如前面的identity对象会在matrix.c中产生出下面的__sti()函数：

```c++
//编译器的行为
__sti__matrix_c__identity()
{
    identity.Matrix::Matrix();  //静态初始化
}
```

- 2.在每一个需要静态的内存内存释放操作的文件中，产生一个__std()函数，内含必要的析构函数调用操作，或是析构函数的内联扩展
- 3.提供一组runtime library "munch"函数：个`_main()`函数（用以调用可执行文件中的所有`__sti()`函数）,以及一个exit()函数（以类似方式调用所有的_`_std()`函数）

![](../../pics/language/Inside_the_C++_Object_Model/静态变量的初始化操作.png)

**注意**：C语言不为静态变量自动设定初值

**被静态初始化的对象的缺点**：

- 1.如果支持异常处理，被静态初始化的对象不能放在try代码块内（//TODO:原因？）
- 2.为了控制”需要跨越模块做静态初始化“的objects的相依顺序，而扯出来的复杂度

**作者建议**：

- 不使用需要自动静态初始化的全局对象

### 6.1.2 局部静态对象

```c++
const Matrix& identity()
{
    static Matrix mat_identity;
    //...
    return mat_identity;
}
```

- mat_identity的constructor必须只能执行一次，虽然上述函数可能被调用多次
- mat_identity的destructor必须只能执行一次，虽然上述函数可能会被调用多次

编译器的做法：

- 1.无条件地在程序起始时构造出对象来。（缺点：导致所有的局部静态对象都在程序起始时被初始化，即使它们所在的那个函数从不被调用）
- 2.identity()被调用时才把mat_identity构造起来

cfront实现的方法是

- 1.**构造**：导入一个临时性对象以保护mat_identity的初始化操作。第一次调用identity()时，这个临时对象为false，于是构造函数会被调用，然后临时对象被改为true，再次调用identity()，由于临时对象为true，因此不会再次构造
- 2.**析构**：析构函数也需要有条件地实施于mat_identity，只有在mat_identity已经被构造起来才调用，可以通过临时对象是否为true来判断mat_identity是否已经构造

### 6.1.3 对象数组

```c++
Point knots[10];
```

如果Point没有定义一个构造函数和析构函数，那么上述代码执行的工作与建立一个“内建类型所组成的数组”一样多，只会分配足够内存以存储10个连续的Point元素，但不会构造Point对象

如果Point**定义了默认构造函数**，在cfront编译器中，使用一个命名为vec_new()的函数，产生出以类对象构造而成的数组。（比较新的编译器，则是提供两个函数，一个用来处理”没有虚基类“的类，另一个用来处理”内含虚基类“的类，后一个函数通常被称为vec_vnew()）

函数vec_new()类型通常如下：

```c++
void* vec_new(
    void *array,                        //数组起始地址
    size_t elem_size,                   //每一个class object的大小
    int elem_count,                     //数组中的元素个数
    void (*constructor)(void*),
    void (*destructor)(void*,char)
    )
```

- constructor是类的默认构造函数的函数指针
- destructor是类的默认析构函数的函数指针
- array的值：
  - 具体的数组地址，表示静态数组，即knots这样的
  - 0，表示动态数组，即使用new操作符生成于heap中的对象数组

vec_new的具体使用：

```c++
Point knots[10];
//编译器的行为
vec_new(&knots,sizeof(Point),10,&Point::Point,0);
```

对于提供了一个或兑个显示初值的类对象数组：

```c++
Point knots[ 10 ] = { Point(), Point( 1.0, 1.0, 0.5 ), -1.0 };
//编译器的行为
//显示初始化前3个元素
Point::Point( &knots[0] );
Point::Point( &knots[1], 1.0, 1.0, 0.5 );
Point::Point( &knots[2], -1.0, 0.0, 0.0 );
//以vec_new初始化后面7个元素
vec_new( &knots+3, sizeof( Point ), 7, &Point::Point, 0 );
```

如果Point也定义了**析构函数**，当knots的生命结束时，该析构函数也必须实施于10个Point元素身上，经由一个类似的vec_delete()的runtime library函数完成：

```c++
void* vec_delete(
    void *array,                        //数组起始地址
    size_t elem_size,                   //每一个class object的大小
    int elem_count,                     //数组中的元素个数
    void (*destructor)(void*,char)
    )
```

## 6.2 new和delete运算符

### 6.2.1 new

```c++
int *pi = new int(5);
```

实际上是由两个步骤完成的：

```c++
//通过适当的new运算符函数实例，配置所需内存
//这里的new就是下文提到的operator new
int *pi = __new(sizeof(int));
//将配置得来的对象设置初值
*pi = 5;
```

更进一步，初始化操作应该在内存配置成功后才执行：

```c
int *pi;
if(pi = __new(sizeof(int))) 
    *pi = 5;
```

对类对象的情况：

```c++
Point3d *origin = new Point3d;

//编译器的行为，无异常处理的情况
Point3d *origin;
if( origin = __new( sizeof( Point3d ) ) )
    origin = Point3d::Point3d( origin );

//编译器的行为，有异常处理的情况
Point3d *origin;
if( origin = __new( sizeof( Point3d ) ) )
{
	try{
        origin = Point3d::Point3d( origin );
    }catch(...)
    {
       //如果出现异常
        //调用__delete以释放new分配的内容
        __delete( origin );
    	//将异常上上层传递
        throw;
    }
}
   
```

### 6.2.2 delete

```c++
delete pi;
```

delete pi时，如果pi是0，C++要求delete运算符不要有操作。因此”编译器“必须为此调用构造一层保护

```c++
//保护
if(pi != 0)
    //释放内存，但是pi并不会设为为0
    //__delete就是下文提到的operator delete
    __delete(pi);   
```

对类对象的情况：

```c++
delete origin;

//编译器的行为，无异常处理的情况
if( origin != 0 )
{
    Point3d::~Point3d( origin );
    __delete( origin );
}

//编译器的行为，有异常处理的情况
if( origin != 0 )
{
    try{
        oint3d::~Point3d( origin );
    }catch(...)
    {
        __delete( origin );
        //将异常上上层传递
        throw;
    }
}
```

### 6.2.3 operator new

以下版本并未考虑异常处理

```c++
extern void* operator new(size_t size)
{
    if(size == 0)
        size = 1;

    void *last_alloc;
    while(!(last_alloc = malloc(size))){
        //出错处理
        //允许使用者提供一个属于自己的_new_handler()函数
        if(_new_handler)
            (*_new_handler)();
        else
            return 0;
    }
    return last_alloc;
}
```

```c++
//是合法的
//但语言要求每一次对new的调用都必须传回一个独一无二的指针。解决此问题的传统方法是传回一个指针，指向一个默认为1 byte的内存区块（上述代码中将size设为1的原因）
new T[0];
```

operator new实际上总是以标准的C malloc完成，但不一定得这么做

### 6.2.4 operator delete

以下版本并未考虑异常处理

operator delete也总是以标准的C free()完成

```c++
extern void operator delete(void *ptr)
{
    if(ptr)
        free((char*)ptr);
}
```

### 6.2.5 针对数组的new语意

```c++
int *p_array = new int[5];

//编译器的行为
int *p_array = (int*)__new(5 * sizeof(int));
```

vec_new()不会真正被调用，vec_new()的主要功能是把默认构造函数施行于数组中的每个类对象中，但int类型没有默认构造函数。这里只会调用operator new分配空间

```c++
//struct simple_aggr{float f1,f2;};
simple_aggr *p_aggr = new simple_aggr[5];
```

vec_new()也不会被调用。因为simple_aggr并没有定义一个构造函数或析构函数，所以配置数组以及清除p_aggr数组的操作，只是单纯地获得内存和释放内存。由operator new和operator delete来完成

如果class定义了一个默认构造函数，某些版本的vec_new()就会被调用

```c++
Point3d *p_array = new Point3d[10];

//编译器的行为
Point3d *p_array;
p_array = vec_new( 0, sizeof( Point3d ), 10, &Point3d::Point3d, &Point3d::~Point3d );
```

当 delete 一个指向数组的指针时，C++2.0版之前，需要提供数组的大小。而2.1版后，不需要提供数组大小，只有在 `[]` 出现时，编译器才寻找数组的维度。否则它便假设只有单独一个object要被删除

```c++
//C++2.0版之前
int array_size = 10;
Point3d *p_array = new Point3d[ array_size ];
delete[ array_size ] p_array;

//只有第一个元素会被析构，其他元素仍然存在
delete p_array;
//正确的写法
delete[] p_array;
```

C++2.1之后，delete不需要提供数组大小，如何**记录数组**的元素？有两种方法：

- 1.为vec_new()所传回的每一个内存区块配置一个额外的word，然后把元素个数包藏在这个word之中，通常这种被包藏的数值称为cookie
- 2.Jonathan和Sun编译器维护一个”联合数组“，放置指针及大小。Sun同时也把析构函数的地址放置在“联合数组”中

**cookie策略的问题**：

- 如果一个坏指针被交给delete_vec()，取出来的cookie自然是不合法的。一个不合法的元素个数和一个坏指针的起始地址，会导致析构函数以非预期的次数被实施于一段非预期的区域
- ”联合数组“的策略下，坏指针的可能结果就只是取出错误的元素个数而已

**避免以一个基类指针指向一个派生类对象所组成的数组（如果派生类对象比基类对象大的话）**

```c++
Point *ptr = new Point3d[10];
delete[] ptr;
```

**存在的问题**：

- 实施于数组上的析构函数，是根据交给vec_delete()函数的”被删除的指针类型的析构函数“（在本例中就是Point的析构函数），与我们的期望不符
- 每一个元素的大小也一并被传递过去（本例中是Point类对象的大小）
- vec_delete()以“Point的析构函数”和“Point的对象大小”迭代走过每一个数组元素，最终执行失败，原因：执行了错误的析构函数和元素大小。从第一个元素之后，该destructor即被施行于不正确的内存区块中

**解决方法**：

- 1.程序员层面
- 2.析构函数设为虚函数

```c++
//循环delete
for( int ix = 0; ix < elem_count; ++ )
{
    Point3d *p = &((Point3d*)ptr)[ix];
    delete p;
}
```

**VS中的测试**

将析构函数设为虚函数，就不会有问题

析构函数非虚：

```c++
class A {
public:
	A() { cout << "A()" << " "; }
	~A() { cout << "~A()" << " "; }
};

class B : public A {
public:
	B() { cout << "B()" << " "; }
	~B() { cout << "~B()" << " "; }
};
int main(int agrc, char** agrv)
{
	A* p = new B[3];
	delete[] p;
	cout << endl;
	system("pause");
	return 0;
}
//输出：A() B() A() B() A() B() ~A() ~A() ~A()

//虚析构函数
class A {
public:
	A() { cout << "A()" << " "; }
	//虚析构函数
	virtual ~A() { cout << "~A()" << " "; }
};

class B : public A {
public:
	B() { cout << "B()" << " "; }
	~B() { cout << "~B()" << " "; }
};
int main(int agrc, char** agrv)
{
	A* p = new B[3];
	delete[] p;
	cout << endl;
	system("pause");
	return 0;
}
//输出：A() B() A() B() A() B() ~B() ~A() ~B() ~A() ~B() ~A()
```

## 6.2.6 Placement Operator new的语意

Placement Operator new：一个预定义好的重载的operator new，它需要两个参数

```c++
void* operator new( size_t, void *p );
//使用
//arena指向一块内存区域，返回根据Point2w构造函数构造好的指向arena的Point2w类型的指针
Point2w *ptw = new ( arena ) Point2w;

//编译器的行为
//1.将arena void* 转换为 Point2w*
Point2w *ptw = ( Point2w* ) arena;
//2.将Point2w构造函数实施于arena所指向的地址
if( ptw != 0 )
    ptw->Point2w::Point2w();
```

**问题1：析构**

如果placement operator在原已存在的一个对象上构造新的对象，而原已存在的对象的类有析构函数，但这个函数不会自动被调用，需要手动调用其析构函数

```c++
//错误的析构方式，不仅析构，还释放掉了内存
delete ptw;
ptw = new (arena) Point2w;

//正确的析构方式，只析构，不释放
ptw->~Point2w;
ptw = new (arena) Point2w;
```

由于无法得知arena所指向的区域是否需要析构，所以这一切都需要程序员负责，编译器不会自动执行

**问题2：真正的指针类型**

arena指向的内存：

- 1.必须是相同类型的
- 2.一块“新鲜”内存，并且足够荣内该类型的对象

```c++
//相同类型的
Point2w *arena = new Point2w;

//“新鲜”的存储空间
char* arena = new char[ sizeof( Point2w ) ];

//之后再执行placement operator new
Point2w* ptw = new (arena) Point2w;
```

**问题3：多态**

placement operator new并不支持多态，被交给new的指针，应该适当地指向一块预先配置好的内存

如果派生类比其基类大，如：

```c++
Point2w *p2w = new ( arena ) Point3w; 
```

Point3w的构造函数将导致严重的破坏，因为arena指向的内存只有Point2w对象的大小

如果派生类与基类一样大，如：

```c++
struct Base { int j; virtual void f(); };
struct Derived : Base { void f(); };

void() fooBar
{
    Base b;
    b.f(); //使用对象调用，静态调用虚函数，Base::f()被调用
    b.~Base(); 
    new ( &b ) Derived; 
    b.f(); // 哪一个f()被调用？
}
```

不能确切的说哪一个f()被调用，使用者可能以为调用的是Derived::f()，但大部分编译器调用的是Base::f()

VS中的结果：

```c++
struct Base { int j; virtual void f() { cout << "Base"<<" "; } };
struct Derived : Base { void f() { cout << "Derived"<<" "; } };

void fooBar()
{
	Base b;
	b.f(); //使用对象调用，静态调用虚函数，Base::f()被调用
	b.~Base();
	new (&b) Derived;
	b.f(); // 哪一个f()被调用？
}

int main(int agrc, char** agrv)
{
	fooBar();
	cout << endl;
	system("pause");
	return 0;
}
//输出：Base Base
```

## 6.3 临时对象



