C++

[TOC]

# 关键字

## 1.extern关键字作用

- 1).extern声明变量在文件外部定义？

- 2).extern修饰函数？

- 3).extern C的作用？用法？

extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外extern也可用来进行链接指定。

extern有两个作用：

- 1.当它与"C"一起连用时，如: `extern "C" void fun(int a, int b);`则告诉编译器在编译fun这个函数名时按着C的规则去翻译相应的函数名而不是C++的
- 2.当extern不与"C"在一起修饰变量或函数时，如在头文件中: `extern int g_Int;`它的作用就是声明函数或全局变量的作用范围的关键字，其声明的函数和变量可以在本模块或其他模块中使用，记住它是一个声明不是定义!也就是说B模块(编译单元)要是引用模块A(编译单元)中定义的全局变量或函数时，它只要包含A模块的头文件即可,在编译阶段，模块B虽然找不到该函数或变量，但它不会报错，它会在连接时从模块A生成的目标代码中找到此函数。
- 3.与不使用extern时的区别

```
假如a.h中有 int a=10; t1.cpp和t2.cpp同时include "a.h"则编译不成功，因为a重复定义；
如果 a.h中是 static int a=10;则可以，因为t1和t2中的a只是名字相同，地址空间不同；
如果a.h中是 extern int a; 并且在a.cpp中 int a=10; 则t1和t2中的a指向同一个地址空间。
```

- 4.extern和static不能同时修饰一个变量，链接属性是矛盾的
- 5.extern 和const，没什么特别的，就是他们属性的总和

**参考** 

-	[C/C++中extern关键字详解](https://www.cnblogs.com/yc_sunniwell/archive/2010/07/14/1777431.html)

---

## 2.static关键字作用

- 1).static修饰局部变量？
- 2).static全局变量？(限定变量在一个编译单元内，一个编译单元就是指一个cpp和它包含的头文件，这个回答可以结合编译需要经历的几个过程来答)
- 3).static修饰普通函数？
- 4).static修饰成员变量?
- 5).static修饰成员函数？

```
代码区
全局数据区 <---全局变量、静态全局变量、静态局部变量、静态数据成员
堆区 <--- new
栈区 <--- 函数内部的自动变量
```

C++的static有两种用法：面向过程程序设计中的static和面向对象程序设计中的static。前者应用于普通变量和函数，不涉及类；后者主要说明static在类中的作用。

**面向过程设计中的static**

- 1).静态全局变量：相较于全局变量：不能被其它文件所用，不会与其他文件变量名冲突。
- 2).静态局部变量：相较于局部变量：函数退出时不会自动销毁，自动初始化
- 3).静态函数：静态函数与普通函数不同，它只能在声明它的文件当中可见，不能被其它文件使用


**面向对象的static关键字（类中的static关键字）**  

- 1).静态数据成员：
  - 静态数据成员在程序中也只有一份拷贝，静态数据成员是该类的所有对象所共有的
  - 不能在类声明中定义（int类型的可以，包括char、long一类）
  - ＜数据类型＞＜类名＞::＜静态数据成员名＞=＜值＞  `int Myclass::Sum=0;`
  - 出现在变量定义不能指定关键字static

- 2).静态成员函数
  - 与普通函数相比，静态成员函数由于不是与任何的对象相联系，因此它不具有this指针	
  - 它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，它只能调用其余的静态成员函数
  - 出现在类体外的函数定义不能指定关键字static
  - 非静态成员函数可以任意地访问静态成员函数和静态数据成员
  - 静态成员函数不能访问非静态成员函数和非静态数据成员（因为没有this指针）
  - 静态函数可以调用所属类的构造函数（因为调用构造函数的时候不需要使用this指针，构造函数只是新生成了一个对象）

**参考** 

-	[C++中的static关键字的总结](https://www.cnblogs.com/BeyondAnyTime/archive/2012/06/08/2542315.html)

---

## 3.volatile关键字作用

[volatile关键字](http://www.cppblog.com/mzty/archive/2006/08/08/10959.html) 

volatile关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素更改，比如：操作系统、硬件或者其它线程等。遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问。 

当要求使用volatile 声明的变量的值的时候，系统总是重新从它所在的内存读取数据，即使它前面的指令刚刚从该处读取过数据。而且读取的数据立刻被保存。 

```c++
volatile int i=10;
int a = i;
...
//其他代码，并未明确告诉编译器，对i进行过操作

int b = i;
```

volatile  指出  i是随时可能发生变化的，每次使用它的时候必须从i的地址中读取，因而编译器生成的汇编代码会重新从i的地址读取数据放在b中。而优化做法是，由于编译器发现两次从i读数据的代码之间的代码没有对i进行过操作，它会自动把上次读的数据放在b中。而不是重新从i里面读。这样以来，如果i是一个寄存器变量或者表示一个端口数据就容易出错，所以说volatile可以保证对特殊地址的稳定访问。 

- 1).访问寄存器要比访问内存要块，因此CPU会优先访问该数据在寄存器中的存储结果，但是内存中的数据可能已经发生了改变，而寄存器中还保留着原来的结果。为了避免这种情况的发生将该变量声明为volatile，告诉CPU每次都**从内存去读取数据**。
- 2).一个参数可以即是const又是volatile的吗？可以，一个例子是只读状态寄存器，是volatile是因为它可能被意想不到的被改变，是const告诉程序不应该试图去修改他

---

## 4.const关键字的作用

**使用：** 

- 1).定义常量

- 2).指针使用CONST：指针常量、指向常量的指针

- 3).函数中使用CONST
  - const修饰函数参数
  - const修饰函数返回值：一般用于重载操作符
    - 防止类似`if(a+b==1)`写成`if(a+b=1)`而编译器没发现错误的情况
    - [c/c++: c++函数返回类型什么情况带const](http://www.cnblogs.com/Azhu/p/4352613.html)

- 4).类相关CONST

  - const修饰成员变量：它只能在初始化列表中赋值（与static搭配则在类外初始化）
  - const修饰成员函数：const成员函数不被允许修改它所在对象的任何一个数据成员，即this指针是const的；

     - const成员函数能够访问对象的const成员，而其他成员函数不可以；
     - 对于const类对象/指针/引用，只能调用类的const成员函数，因此，const修饰成员函数的最重要作用就是限制对于const对象的使用
  - const修饰对象指针/对象引用：
     - 表示该对象为常量对象，其中的任何成员都不能被修改；
     - 任何非const成员函数都不能被调用


**初始化类内部的常量** 

- 1).static与const并用，在外部初始化（const int A::a = 10;）
- 2).初始化列表

**将Const类型转化为非Const类型的方法** 

`const_cast <type_id> (expression) `

- 常量指针被转化成非常量指针，并且仍然指向原来的对象
- 常量引用被转换成非常量引用，并且仍然指向原来的对象

**参考** 

-	[关于C++ const 的全面总结](http://blog.csdn.net/Eric_Jo/article/details/4138548)

## 5.final关键字的作用

- 1.禁用继承

```c++
class Super final  
{  
  //......  
}; 
```

- 2.禁用重写，C++中还允许将方法标记为fianal，这意味着无法在子类中重写该方法。这时final关键字至于方法参数列表后面。只能对虚函数使用

```c++
class Super  
{  
public:  
	Supe();  
    virtual void SomeMethod() final;  
};  
```

## 6.typedef与using

[typedef与using、struct与class的区别](https://blog.csdn.net/github_35681219/article/details/52744537)

- 定义一般类型的别名没区别，都是用来简化代码。如`typedef string::size_type str_sz`，将`string::size_type`类型命名为`str_sz`，类型名在前，别名在后；而using的用法为`using str_sz=string::size_type`，别名在前，类型名在后；
- 定义模板的别名，只能使用using。如：`using cell = pair<void*, cell*>`

using的其他用途：

- 1.在当前文件中引入命名空间


- 2.在子类中使用using 声明引入基类成员名称
  - 可用于函数重载：让父类同名函数在子类中以重载方式使用，可以参见下面“函数重载”部分
- 3.继承构造函数

```c++
struct A
{
  A(int i) {}
  A(double d,int i){}
  A(float f,int i,const char* c){}
  //...等等系列的构造函数版本
};
struct B:A
{
  using A::A;
  //类成员的初始化表达式，为派生类成员设定一个默认初始值
  int d{0};
};
```

[C++ using关键字作用总结](http://www.cnblogs.com/ustc11wj/archive/2012/08/11/2637316.html)

[C++11中的继承构造函数](https://blog.csdn.net/u012333003/article/details/32348297)

## 7.class与struct的区别

- 1.默认的数据成员访问权限
  - struct是public的
  - class是private的
- 2.class可用于定义模板参数，就像“typename”。但struct不用于定义模板参数

## 8.访问控制说明符

- [访问权限说明符](https://github.com/arkingc/note/blob/master/C++/访问控制说明符.md)（目的是加强类的封装性）
- 1.**类的成员的访问控制说明符**用于控制**类的使用者**对类中成员的访问权限
- 2.**派生列表中的访问控制说明符**用于控制**派生类的使用者**对**派生类从基类继承的成员**的访问权限

![](../../../pics/WangDao/继承中的访问权限.png)

## 9.explict

禁止构造函数的隐式转换

在C++中，explicit关键字用来修饰类的构造函数，被修饰的构造函数的类，不能发生相应的隐式类型转换，只能以显示的方式进行类型转换

注意事项：

- explicit 关键字只能用于类内部的构造函数声明上
- explicit 关键字作用于**单个参数**的构造函数
- explicit关键字用来修饰类的构造函数，被修饰的构造函数的类，不能发生相应的隐式类型转换

[C++  explicit的作用](http://www.cnblogs.com/this-543273659/archive/2011/08/02/2124596.html)

## 10.mutable

[C++关键字mutable](http://www.cnblogs.com/danshui/archive/2012/01/05/2313647.html)

mutable的意思是“可变的，易变的”，跟C++中的const是反义词

用法：

- 1.const成员函数：在C++中，mutable也是为了突破const的限制而设置的。被mutable修饰的变量，将永远处于可变的状态，即使在一个const函数中

```c++
class TestMutable
{
public:
    TestMutable(){i=0;}
    int Output() const
    {
        return i++; //合法
    }
private:
    mutable int i;
};
```

- 2.lambda：mutable指示符：用来说用是否可以修改捕获的变量

```c++
void fcn3()
{
　　size_t v1=42; //局部变量
　　//f可以改变它所捕获的变量的值
　　auto f=[v1] () mutable {return ++v1;};
　　v1=0;
　　auto j=f(); //j为43
}
```

## 11.noexcept

[《C++Primber》笔记 第IV部分](https://guanjunjian.github.io/2017/02/09/study-23-cpp-primer-summary_4/)

- noexcept说明符和noexcept运算符
  - 跟在函数参数列表后是异常说明符
  - 作为noexcept异常说明的bool实参出现时，是一个运算符

> noexcept说明符

noexcept说明指定某个函数不会抛出异常。如果说明noexcept的同时又含有throw语句，一旦一个noexcept函数抛出了异常，程序就会调用terminate以确保遵守不在运行时抛出异常的承诺。

noexcept说明符接受一个可选的实参，该实参必须能转换为bool类型。

- 如果实参是true，则函数不会抛出异常
- 如果实参是false，则函数可能抛出异常

```c++
void recoup(int) noexcept;  // 不会抛出异常
void alloc(int);        // 可能抛出异常
void recoup(int) noexcept(true);    // recoup不会抛出异常
void alloc(int) noexcept(false);    // alloc可能抛出异常
```

> noexcept运算符

一个一元运算符，返回值是一个bool类型的右值常量表达式，用于表示给定的表达式是否会抛出异常，运算符不会求其运算对象的值

```c++
//如果recoup不抛出异常则结果为true；否则结果为false
noexcept(recoup(i));
//当e调用的所有函数都做了不抛出说明且e本身不含有throw语句时，返回true，否则false
noexcept(e)
```

## 12.default、delete

[C++中的默认函数与default和delete用法](https://blog.csdn.net/u012333003/article/details/25299939)

- default显示地指示编译器生成某函数的默认版本（如构造函数、一些操作符）

```c++
class MyClass
{
  public:
    MyClass()=default;  //同时提供默认版本和带参版本，类型是POD的
    MyClass(int i):data(i){}
  private:
    int data;
};
```

- 使用delete关键字显式指示编译器不生成函数的默认版本（如复制构造函数）
  - 可用于阻止拷贝，此时使用了delete
  - 阻止拷贝的外方式：设为private，且只声明不定义

```c++
class MyClass
{
  public:
     MyClass()=default;
     MyClass(const MyClass& )=delete;
  ......
}
```

## 13.final、override

[c++11特性之override和final关键字 ](https://blog.csdn.net/wangshubo1989/article/details/49539251)

**final**

- 1.阻止类的进一步派生
- 2.阻止虚函数的进一步重写

```c++
class TaskManager final {/*..*/} ; 
class  B: A
{
pulic:
  void func() const override final; //OK
};
```

**override**

- 1.确保在派生类中声明的重写函数跟基类的虚函数有相同的签名

```c++
virtual void func(double) override;
```

## 14.auto

[auto的使用](https://blog.csdn.net/huang_xw/article/details/8760403)

- 1.自动类型推断
- 2.返回值占位

```c++
template <typename T1, typename T2>  
auto compose(T1 t1, T2 t2) -> decltype(t1 + t2)  
{  
   return t1+t2;  
}  
```

- 注意事项：
  - 可以使用valatile，pointer（*），reference（&），rvalue reference（&&） 来修饰auto
  - 用auto声明的变量必须初始化
  - auto不能与其他类型组合连用
  - 函数和模板参数不能被声明为auto
  - 定义在堆上的变量，使用了auto的表达式必须被初始化
  - 不能用于类型转换或其他一些操作，如sizeof和typeid
    - 即不能使用`auto x2 = (auto)value`
  - 定义在一个auto序列的变量必须始终推导成同一类型
  - auto不能自动推导成CV-qualifiers（constant & volatile qualifiers），除非被声明为引用类型
  - auto会退化成指向数组的指针，除非被声明为引用

```c++
//堆的问题
auto* y = new auto(9); // Fine. Here y is a int*

const int i = 99;  
auto j = i; 
j = 100; //正确，因为j为int
    
auto& k = i;
k = 100; //错误，因为k位const int&

//关于数组的指针退化的例子
int a[9];
auto j = a;
cout<<typeid(j).name()<<endl; // This will print int*
auto& k = a;
cout<<typeid(k).name()<<endl; // This will print int [9]
```

## 15.decltype

[auto 和 decltype 区别和联系](https://blog.csdn.net/y1196645376/article/details/51441503)

decltype，它的作用是选择并返回操作数（变量或表达式）的数据类型，编译器只是分析表达式并得到它的类型，却不进行实际的计算表达式的值

```c++
decltype(f()) sum = x;// sum的类型就是函数f的返回值类型。 
```

**decltype与auto的区别**

- decltype在处理顶层const和引用的方式与auto有些许不同，如果decltype使用的表达式是一个变量，则decltype返回该变量的类型(包括顶层const和引用在内)。
  - 但指针的底层const的处理是一样的，就是都会把底层const拿过来
  - 引用的底层const，auto不会拿过来

```c++
    const int ci = 42, &cj = ci;  
      
    decltype(ci) x = 0;   // x 类型为const int  
    auto z = ci;          // z 类型为int  
      
    decltype(cj) y = x;   // y 类型为const int&  
    auto h = cj;          // h 类型为int  
```

- decltype的结果类型与表达形式密切相关，对于decltype 所用表达式来说，如果变量名加上一对括号，则得到的类型与不加上括号的时候可能不同。
  - 不加括号的变量，那么得到的结果就是这个变量的类型
  - 这个变量加上一个或多层括号，那么编译器会把这个变量当作一个表达式看待，变量是一个可以作为左值的特殊表达式，所以这样的decltype就会返回引用类型

```c++
int i = 42;

//decltype(i)   int  类型
//decltype((i)) int& 类型
```

## 16.typeid

如果表达式的类型是类类型，且类至少包含有一个虚函数，则typeid操作符返回表达式的动态类型，需要在运行时计算；否则，typeid操作符返回表达式的静态类型，在编译时就可以计算

```c++
class A {
public:
	virtual void fun() {};
};
class B :public A{};

int main()
{
	A* pa = new B();
    //输出class B
	cout << typeid(*pa).name();
	system("pause");
	return 0;
}

----------------------------------------

class A {
public:
	//virtual void fun() {};
};
class B :public A{};

int main()
{
	A* pa = new B();
    //输出class A
	cout << typeid(*pa).name();
	system("pause");
	return 0;
}
```

## 17.如何修改const变量、const与volatile、常量折叠

[C++中如何修改const变量](https://blog.csdn.net/heyabo/article/details/8745942)

[const变量通过指针修改 详解](https://blog.csdn.net/hyqsong/article/details/50867456)

const只是编译时期的一种强类型安全检查机制的一种手段，他只是帮组我们发现无意中修改的常量，无法在二进制层面进行保护 

- const全局变量存储在全局存储空间，其值只有可读属性，不能修改 
- const局部变量存储在堆栈中，可通过指针修改其值； 
- 但const变量在预处理时处理，编译器只对其值读取一次，因此如果想要真正能const变量的值改变，需要使用volatile关键字

```c++
#include <stdio.h>
int main()
{
    const volatile int i = 10;
    int* pi = (int*)(&i);
    *pi = 100;
    printf("*pi: %d\n",*pi);
    printf("i: %d\n",i);
    printf("pi: %p\n",pi);
    printf("&i: %p\n", &i);
    return 0;
}
```

const如果不使用volatile即使使用指针方式修改后，也不能打印出改后值的原因：**常量折叠** 

- 常量折叠的概念：在编译器里进行语法分析的时候，将常量表达式计算求值，并用求得的值来替换表达式，放入常量表，可以算作一种编译优化 

- 可折叠的常量像宏一样，在预编译阶段对常量的引用一律被替换为常量所对应的值 
- 常量折叠说的是，在编译阶段，对该变量进行值替换，同时，该常量拥有自己的内存空间，并非像宏定义一样不分配空间[C++常量折叠](https://blog.csdn.net/yby4769250/article/details/7359278)

# 内存和动态内存

## 1.new与malloc区别

- 1).分配单位：new分配内存按照数据型进行分配，malloc分配内存按照大小分配，以字节为单位；
- 2).构造、析构：new不仅分配一段内存，而且会调用构造函数，但是malloc则不会。new分配的内存要用delete销毁，malloc要用free来销毁；delete销毁的时候会调用对象的析构函数，而free则不会
- 3).返回值：new返回的是指定对象的指针，而malloc返回的是void*，因此malloc的返回值一般都需要进行类型转化
- 4).库和操作符：new是一个操作符可以重载，malloc是一个库函数
- 5).扩容：malloc分配的内存不够的时候，可以用realloc扩容。扩容原理：如果原先的内存无法改变大小，realloc将分配另一块正确大小的内存，并把原先那块内存的内容复制到新的块上

```
realloc原型是extern void *realloc(void *mem_address, unsigned int newsize);
指针名=（数据类型*）realloc（要改变内存大小的指针名，新的大小）
```

- 6).异常：new如果分配失败了会抛出bad_malloc的异常（可以用new(nothrow)来不抛出），而malloc失败了会返回NULL。因此对于new，正确的姿势是采用try…catch语法，而malloc则应该判断指针的返回值。为了兼容很多c程序员的习惯，C++也可以采用new nothrow的方法禁止抛出异常而返回NULL；
- 7).动态数组：new和new[]的区别，new[]一次分配所有内存，多次调用构造函数，分别搭配使用delete和delete[]，同理，delete[]多次调用析构函数，销毁数组中的每个对象。而malloc则只能sizeof(int) * n；

**TODO** 

new和malloc的实现、空闲链表，分配方法(首次适配原则，最佳适配原则，最差适配原则，快速适配原则)。delete和free的实现原理，free为什么直到销毁多大的空间

**参考** 

-	[new与malloc的前世今生](http://blog.csdn.net/shanghairuoxiao/article/details/70337890)

## 2.内存对齐的原则

- 对其的好处：对齐节省了时间

比如对于int x;（这里假设sizeof(int)==4），**因为cpu对内存的读取操作是对齐的**，如果x的地址不是4的倍数，那么读取这个x，需要读取两次共8个字节，然后还要将其拼接成一个int，这比存取对齐过的x要麻烦很多

- 1).数据成员对齐规则：结构(struct)(或联合(union))的数据成员，第一个数据成员放在offset为0的地方，以后每个数据成员存储的起始位置要从该成员大小或者成员的子成员大小（只要该成员有子成员，比如说是数组，结构体等）的整数倍开始(比如int在32位机为4字节,则要从４的整数倍地址开始存储。
- 2).结构体作为成员:如果一个结构里有某些结构体成员,则结构体成员要从其内部最大元素大小的整数倍地址开始存储.(struct a里存有struct b,b里有char,int ,double等元素,那b应该从8的整数倍开始存储.)
- 3).收尾工作:结构体的总大小,也就是sizeof的结果,.必须是其内部最大成员的整数倍.不足的要补齐

```c
typedef struct bb
{
 int id;             //4字节，[0]....[3]
 double weight;      //8字节，[8].....[15]，原则1：从8的整数倍开始
 float height;      //4字节，[16]..[19],总长要为8的整数倍,补齐[20]...[23]，原则3：补齐最大最大成员的整数倍
}BB;

typedef struct aa
{
 char name[2];     //2字节，[0],[1]
 int  id;         //4字节，[4]...[7]，原则１：从4的整数倍开始

 double score;     //8字节，[8]....[15]　　　　
 short grade;    //2字节，[16],[17]　　　　　　　　
 BB b;             //[24]......[47]，原则２：从BB结构体中最大成员，也就是double，8字节，的整数倍开始
}AA;

int main()
{
  AA a;
  cout<<sizeof(a)<<" "<<sizeof(BB)<<endl;
  return 0;
}

//输出结果为48 24
```

- 4).\#pragma pack()
  - \#pragma pack(1)：告诉编译器,所有的对齐都按照1的整数倍对齐

如果加入pragma pack(n) ，取n和变量自身大小较小的一个进行对齐

**总结：** 

- 1).从0位置开始存储
- 2).变量存储的起始位置是该变量大小的整数倍
- 3).结构体总的大小是其最大元素的整数倍，不足的后面要补齐
- 4).结构体中包含结构体，从结构体中最大元素的整数倍开始存
- 5).如果加入pragma pack(n) ，取n和变量自身大小较小的一个


[5分钟搞定内存字节对齐](https://blog.csdn.net/hairetz/article/details/4084088)

## 3.C++内存管理

[C/C++内存管理详解-与1) 2)5)相关](https://chenqx.github.io/2014/09/25/Cpp-Memory-Management/)

[C/C++内存管理详解---与3)相关](http://zheming.wang/blog/2012/03/23/36E28580-280D-4708-BEAF-B3A3E5107377/)

> 1).C++内存分为那几块,每块存储哪些变量

- a.栈（stack）:由编译器自动分配释放，存放函数的参数值、局部变量的值等，其操作方式类似于数据结构的栈。（分为：静态分配：一般局部变量；动态分配：alloc函数）
- b.堆（heap）：一般是由程序员分配释放，若程序员不释放的话，程序结束时由OS回收，值得注意的是它与数据结构的堆是两回事，分配方式倒是类似于数据结构的链表。用new、malloc， calloc， realloc等内存分配函数分配的内存就是在堆上
- c.全局/静态区（static）：存储全局变量和静态变量，全局变量和静态变量被分配到同一块内存中，在以前的C语言中，全局变量又分为初始化的和未初始化的，在C++里面没有这个区分了，他们共同占用同一块内存区。
- d.文字常量区:常量字符串就是放在这里，程序结束后由系统释放。关于文字常量区存放位置，视编译器
  - 专用的字符串池内存块
  - 静态数据区
  - 代码区
- e.程序代码区（.data）：存放函数体的二进制代码。

![](../../../pics/interview/language/C++/内存分布.png)

> 2).防止堆破碎的方法

一个防止堆破碎的通用方法是从不同固定大小的内存池中分配不同类型的对象（类似slab），对每个类重载operator new 和operator delete。

> 3).堆和栈区别

- (1). 管理方式不同：栈，由编译器自动管理；堆，程序员控制，容易产生内存泄露
- (2). 空间大小不同：32位，堆，不超过3G；栈，内存由限制，VC6大约1M，可设置
- (3). 能否产生碎片不同：堆，频繁的`new/delete`，造成大量的碎片；栈，不会产生碎片
- (4). 生长方向不同：堆：内存地址增加的方向；栈，内存地址减小的方向
- (5). 分配方式不同：堆，动态分配（malloc、new）；栈，动态分配（alloca），静态分配（局部变量）
- (6). 分配效率不同：堆，机制复杂，效率低；栈，效率高



## 4.定位内存泄露 

//TODO

- 1).在windows平台下通过CRT中的库函数进行检测；
- 2).在可能泄漏的调用前后生成块的快照，比较前后的状态，定位泄漏的位置
- 3).Linux下通过工具valgrind检测

[C/C++内存泄漏及检测](http://www.cnblogs.com/skynet/archive/2011/02/20/1959162.html)

## 5.allocator类

> 1).为什么需要allocator类

- new存在灵活上的局限性：将内存分配和对象构造组合在一起
- delete存在灵活上的局限性：将对象析构和内存释放组合在一起
- 将内存分配和对象构造组合在一起可能导致不必要的浪费

> 2).使用方法

![](../../../pics/interview/language/C++/allocator.png)

[《C++Primber》笔记 第Ⅱ部分](https://guanjunjian.github.io/2017/01/26/study-21-cpp-primer-summary_2/)

## 7.全局数组和局部数组的初始化

- 全局数组存储在全局/静态数据区；局部数组存在栈
- 全局不显示初始化，则全初始化为0；局部数组如果不显示初始化，它的值不确定 

---

# 类

## 1.C++多态性与虚函数

> 1).C++多态的实现

多态分为静态多态和动态多态。静态多态（编译时多态）是通过重载和模板技术实现，在编译的时候确定。动态多态（运行时多态）通过虚函数和继承关系来实现，执行动态绑定，在运行的时候确定。

**动态多态实现有几个条件：** 

- (1).虚函数；
- (2).一个基类的指针或引用,指向派生类的对象；

**调用过程：** 

基类指针在调用成员函数(虚函数)时，就会去查找该对象的虚函数表指针。虚函数表指针的地址在每个对象的首地址。查找该虚函数表中该函数的指针进行调用。

每个对象中保存的只是一个虚函数表的指针，C++内部为每一个类维护一个虚函数表（编译时确定），该类的对象的虚函数表指针都指向这同一个虚函数表。

虚函数表中为什么就能准确查找相应的函数指针呢？因为在类设计的时候，虚函数表直接从基类也继承过来，如果覆盖（重写）了其中的某个虚函数，那么虚函数表的指针就会被替换，因此可以根据指针准确找到该调用哪个函数。

[浅谈C++多态性](http://blog.csdn.net/hackbuteer1/article/details/7475622)

> 2).纯虚函数

纯虚函数：是在基类中声明的虚函数，它在基类中没有定义，但要求任何派生类都要定义自己的实现方法。在基类中实现纯虚函数的方法是在函数原型后加“=0” 

```c
virtual void funtion()=0 
```

抽象类：

- 包含纯虚函数的类称为抽象类。由于抽象类包含了没有定义的纯虚函数，所以不能定义抽象类的对象
- 默认构造函数声明为protected、private的类

> 3).静态多态与动态多态的区别

静态多态是指通过模板技术或者函数重载技术实现的多态，其在编译期确定行为。动态多态是指通过虚函数技术实现在运行期动态绑定的技术。

[C++ 静态多态和动态多态 浅析](http://blog.csdn.net/u013630349/article/details/48009815)

> 4).虚函数的作用

- 用于实现多态
- 封装和抽象，如抽象工厂模式

> 5).虚函数表

编译器为每一个类维护一个虚函数表，每个对象的首地址保存着该虚函数表的指针，同一个类的不同对象实际上指向同一张虚函数表。

[C++ 虚函数表解析](http://blog.csdn.net/haoel/article/details/1948051/)

---

## 2.构造函数和析构函数

> 1).为什么对于存在虚函数的类中析构函数要定义成虚函数？

为了实现多态进行动态绑定，将派生类对象指针绑定到基类指针上，指针销毁时，如果析构函数没有定义为析构函数，则只会调用基类的析构函数，显然只能销毁基类的数据。如果要调用派生类的析构函数，就需要将派生类的析构函数定义为虚函数，销毁时通过虚函数表找到对应的析构函数。此时，调用顺序为派生类析构函数、基类析构函数

> 2).析构函数能抛出异常吗

[《Effective C++:条款8》](https://github.com/arkingc/note/blob/master/C++/EffectiveC++.md#%E6%9D%A1%E6%AC%BE08%E5%88%AB%E8%AE%A9%E5%BC%82%E5%B8%B8%E9%80%83%E7%A6%BB%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0) 

不能。

C++标准指明析构函数不能、也不应该抛出异常。C++异常处理模型最大的特点和优势就是对C++中的面向对象提供了最强大的无缝支持。那么如果对象在运行期间出现了异常，C++异常处理模型有责任清除那些由于出现异常所导致的已经失效了的对象(也即对象超出了它原来的作用域)，并释放对象原来所分配的资源， 这就是调用这些对象的析构函数来完成释放资源的任务，所以从这个意义上说，析构函数已经变成了异常处理的一部分。

如果析构函数抛出异常，则异常点之后的程序不会执行，如果析构函数在异常点之后执行了某些必要的动作比如释放某些资源，则这些动作不会执行，会造成诸如资源泄漏的问题。

通常异常发生时，c++的机制会调用已经构造对象的析构函数来释放资源，此时若析构函数本身也抛出异常，则前一个异常尚未处理，又有新的异常，会造成程序崩溃的问题。

> 3).构造函数和析构函数中能调用虚函数吗

[《Effective C++:条款9》](https://github.com/arkingc/note/blob/master/C++/EffectiveC++.md#%E6%9D%A1%E6%AC%BE09%E7%BB%9D%E4%B8%8D%E5%9C%A8%E6%9E%84%E9%80%A0%E5%92%8C%E6%9E%90%E6%9E%84%E8%BF%87%E7%A8%8B%E4%B8%AD%E8%B0%83%E7%94%A8virtual%E5%87%BD%E6%95%B0) 

《C++对象模型》5.2.2 虚函数表指针初始化语意

[构造函数与析构函数中不调用虚函数](http://blog.csdn.net/linpengbin/article/details/51560276)

[【C++】不要在构造函数或析构函数内调用虚函数](https://www.cnblogs.com/vincently/p/4754206.html)

**在子类构造期间，virtual函数绝不会下降到派生类阶层。取而代之的是，对象的作为就像隶属基类类型一样。即派生类对象的基类构造期间，对象的类型是基类而不是派生类；除此之外，若使用运行期类型信息**（如dynamic_cast和typeid），也会把对象视为基类类型（这样对待是合理的：因为子类部分尚未初始化，如果调用的是子类的虚函数，通常会访问子类部分的数据，会引发安全问题） 

**同样的道理也适用于析构函数。一旦派生类析构函数开始执行，对象内的派生类成员变量便呈现未定义值，所以C++视它们仿佛不再存在。进入基类析构函数后对象就成为一个基类对象** 

在构造函数中，虚函数指针**初始化的时机为**： 

- 在基类构造函数调用之后，在初始化列表和用户代码之前 
- 这样可以保证在构造函数和析构函数中调用虚函数的正确性 

```c++
PVertex::PVertex( float x, float y, float z )
    :_next(0), Vertex3d( x,y,z ), Point( x,y )
{
    if( spyOn )
        cerr << "Within PVertex::PVertex()"
        	<< "size: " << size() << endl; //size()为虚函数
}

//编译器的行为
PVertex* PVertex::PVertex( PVertex* this, bool __most_derived, float x, float y, float z )
{
    //如果是最底层的派生类，调用虚基类构造函数
    if(__most_derived!=false)
        this->Point::Point( x,y );
    //无条件地调用直接基类的构造函数
    this->Vertext3d::Vertex3d( x, y, z );
    
    //初始化自身的虚函数表指针，指向正在构建的对象（子对象）的类的虚函数表
    this->__vptr_PVertex = __vtbl_PVertex;
    //初始化虚基类的虚函数表指针
    this->__vptr_Point__PVertex = __vtbl_Point_PVertex;
    
    //用户代码
    if( spyOn )
        cerr << "Within PVertex::PVertex()"
        << "size: "
        //根据正在构建的对象（子对象）的类的虚函数表进行虚函数调用
        << (*this->__vptr__PVertex[ 3 ].faddr)(this)
        << endl;
    return this;
}
```

> 4).必须在构造函数初始化列表里进行初始化的数据成员有哪些

- a.常量成员，因为常量只能初始化不能赋值，所以必须放在初始化列表里面（也可以与static结合，在类外初始化）
- b.引用类型，引用必须在定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面
	 c.没有默认构造函数的类类型，因为使用初始化列表可以不必调用默认构造函数来初始化，而是直接调用拷贝构造函数初始化（或其他构造函数）。如果在初始化列表没有调用，则初始化列表阶段会自动调用类类型变量的默认构造函数，因为该变量没有默认构造函数，则会报错	

> 5).构造函数的初始化列表

构造函数分为两个阶段执行：

- 初始化阶段（初始化列表）：如果初始化列表为空，类类型调用默认构造函数（如果没有则报错），内置类型和复合类型在构建的对象是全局时默认初始化，局部时没有初始化。
- 普通计算阶段（函数体）。

必须在初始化列表中进行初始化的成员：

- 没有默认构造函数的类类型成员
- const类型（与static一起使用时，可以在类外初始化）
- 引用

## 3.复制构造函数

- 1.复制构造函数、复制操作符、析构函数称为复制控制。
- 2.三法则：如果需要析构函数，则需要复制控制的所有成员（三个），因为：
  - 若存在自定义（且正确）的析构函数，说明一般有指针成员，但使用合成的拷贝构造函数，那么拷贝过去的也只是指针，此时两个对象的指针变量同时指向同一块内存，指向同一块内存的后果很有可能是在两个对象中的析构函数中先后被释放两次。所以需要额外的拷贝控制函数去控制相应资源的拷贝。
- 3.一定需要自己定义复制控制的情况：
  - 对于凡是包含动态分配成员或包含指针成员的类都应该提供拷贝构造函数
  - 在提供拷贝构造函数的同时，还应该考虑重载`=`赋值操作符号
- 4.复制构造函数的形式：const+引用：`MyClass(const MyClass &other)`，否则出现循环
- 5.类的两种初始化方式
  - 直接初始化（圆括号）：在对象初始化时，通过括号给对象提供一定的参数，并且要求编译器使用普通的函数匹配来选择与我们提供的参数最匹配的构造函数
    - 直接初始化不一定要调用复制构造函数
  - 复制初始化（=号）：将一个已有的对象拷贝到正在创建的对象，如果需要的话还需要进行类型转换
    - 复制初始化一定要调用复制构造函数
    - 复制初始化发生在下列情况：
      - 使用赋值运算符定义变量（先隐式调用构造函数，再调用复制构造函数）
      - 将对象作为实参传递给一个非引用类型的形参
      - 将一个返回类型为非引用类型的函数返回一个对象
      - 用花括号列表初始化一个数组中的元素或一个聚合类中的成员
- 6.复制初始化和直接初始化的区别：
  - 直接初始化直接调用与**实参匹配**的构造函数，复制初始化总是调用复制构造函数。复制初始化首先使用指定构造函数创建一个临时对象，然后用复制构造函数将那个临时对象复制到正在创建的对象。所以当复制构造函数被声明为私有时（或delete），所有的复制初始化都不能使用。
  - 复制初始化`ClassTest ct3 = ct1`，调用复制构造函数
  - 直接初始化`ClassTest ct4(ct1)`，调用复制构造函数，这个是按照实参匹配调用的
  - 需要注意编译器优化而省略掉的步骤
- 7.复制构造函数的使用场景：
  - 类类型作为实参传给非引用类型形参
  - 类类型作为非引用类型的返回值
  - 初始化顺序容器：先调用默认构造函数，再调用复制构造函数
  - 数组：
    - 如果没有初始化式，调用默认构造函数
    - 有初始化式：`MyClass test[] = { MyClass(1) }`，先调用构造函数，再调用复制构造函数，但有的编译器会优化掉后来那步（编译器的思想是能不用临时对象就不用临时对象）
- 8.浅复制、深复制：引用（怎么实现？）、指针变量有区别
- 9.默认复制构造函数（合成复制构造函数）的行为：
  - 对所有成员变量进行一次赋值操作
  -  因此如果成员的缺省的赋值操作不能满足需求（如是一个链表或含有指针），就必须要对成员类定义重载赋值操作符，才能够让合成拷贝构造函数起作用
- 10.合成复制构造函数的情况：
  - 类成员有：如果一个类没有拷贝构造函数，但是含有一个类类型的成员变量，该类型含有拷贝构造函数，此时编译器会为该类合成一个拷贝构造函数
  - 基类有：如果一个类没有拷贝构造函数，但是该类继承自含有拷贝构造函数的基类，此时编译器会为该类合成一个拷贝构造函数
  - 有虚函数：如果一个类没有拷贝构造函数，但是该类声明或继承了虚函数，此时编译器会为该类合成一个拷贝构造函数
  - 有虚基类：如果一个类没有拷贝构造函数，但是该类含有虚基类，此时编译器会为该类合成一个拷贝构造函数

[C++学习笔记之三五法则](https://blog.csdn.net/u013613341/article/details/50551513)

[入解释直接初始化与复制初始化的区别](https://blog.csdn.net/ljianhui/article/details/9245661)

## 4.拷贝赋值运算符

//TODO

（合成版的行为？、与delete？、自定义时要注意自赋值，参数与返回类型、大部分组合了拷贝构造函数与析构函数的工作）

- 1.赋值操作符的自赋值：返回（*this）
  - 如果是自赋值，则不作任何处理，直接返回 *this；如果不是自赋值，首先释放实例自身已有内存，然后再分配新的内存

```c++
//方法1
Widget& Widget::operator=(cosnt Widget& rhs)
{
    if (this == &rhs) return *this; // identity test: if a self-assignment, do nothing
    
    delete pb;
    pb = new Bitmap(*rhs.pb);
    return *this;
}

//方法2
Widget& Widget::operator=(const Widget& rhs){
    Bitmap *pOrig = pb;               // remember original pb
    pb = new Bitmap(*rhs.pb);         // make pb point to a copy of *pb
    delete pOrig;                     // delete the original pb
    return *this;
}

//方法3
//参数为pass by reference
Widget& Widget::operator=(const Widget &rhs){
    Widget temp(rhs);
    swap(temp);               // swap *this's data with
    return *this;             // the copy's
}
```

## 5.sizeof类

```c++
class A{
    //sizeof(A)输出1
    //空类的大小为1，当类不包含虚函数和非静态数据成员时，其对象大小也为1
    //为了空类实例的identity
    //它有一个隐晦的1字节，是编译器安插的，使用class A的对象得以在内存中配置独一无二的地址
};
//函数
class B{
    int fun1(int);
    int fun2(int);
    //sizeof(B)输出1
};
class C{
    static int fun1(int);
    static int fun2(int);
    //sizeof(C)输出1
};
class D
{
    virtual int fun1(int);
    virtual int fun2(int);
    //sizeof(D)输出4，虚函数表指针
}
class E
{
    friend int fun1(int);
    friend int fun2(int);
    //sizeof(E)输出1
}

//变量
class F{
    int a;
    int b;
     //sizeof(F)输出8
};
class G{
    static int a;
    static int b;
    //sizeof(G)输出1
};

int main()
{
    cout << sizeof(A) <<endl;
    cout << sizeof(B) <<endl;
    cout << sizeof(C) <<endl;
    cout << sizeof(D) <<endl;
    cout << sizeof(E) <<endl;
    cout << sizeof(F) <<endl;
    cout << sizeof(G) <<endl;
    return 0;
}
```

**总结**

- 1.为类的非静态成员数据的类型大小之和．
- 2.由编译器额外加入的成员变量的大小，用来支持语言的某些特性（如：指向虚函数的指针、虚基指针，在VS中，虚函数表指针和虚基类指针是不同的两个指针，但继承多个虚基类时，派生类使用的是同一个虚基类指针）．
- 3.为了优化存取效率，进行的边缘调整（对齐）
- 4.与类中的构造函数，析构函数以及其他的成员函数无关．
- [sizeof() 类大小，空类大小](https://blog.csdn.net/liu_qiqi/article/details/9344627)

```c++
class Base1
{
public:
	virtual void fun1() {}
};

class Base2
{
public:
	virtual void fun2() {}
};

class Derived :public virtual Base1, public virtual Base2
{
public:
	void fun1() override {}
	void fun2() override {}
	void virtual fun3() {}
};

/*
1>class Derived	size(16):
1>	+---
1> 0	| {vfptr}   //由fun3导致的
1> 4	| {vbptr}   //由Base1和Base2的虚继承导致的
1>	+---
1>	+--- (virtual base Base1)
1> 8	| {vfptr}  //由fun1导致的
1>	+---
1>	+--- (virtual base Base2)
1>12	| {vfptr}  //由fun2导致的
1>	+---
1>
1>Derived::$vftable@Derived@:
1>	| &Derived_meta
1>	|  0
1> 0	| &Derived::fun3
1>
1>Derived::$vbtable@:
1> 0	| -4
1> 1	| 4 (Derivedd(Derived+4)Base1)
1> 2	| 8 (Derivedd(Derived+4)Base2)
1>
1>Derived::$vftable@Base1@:
1>	| -8
1> 0	| &Derived::fun1
1>
1>Derived::$vftable@Base2@:
1>	| -12
1> 0	| &Derived::fun2
1>
*/
```

## 6. sizeof派生类 //TODO

## 7.如何防止类被继承

- 1.对类声明final

```c++
    class Super final  
    {  
      //......  
    };  
```

- 2.该类的构造函数声明为私有方法
  - 存在的问题：该类本身不能生成对象了
  - 解决方法：
    - 静态函数
    - 友元函数

```c++
class A  {
public:
	static A makeA() {
		A a;
		return a;
	}
private:
	A() { 
		i = 10;
		cout << i; 
	};
	int i;
};
```

- 3.使用 虚继承 + 私有基类构造函数 + 友元类
  - 这里可以注意到虚继承和一般继承时，超级基类的构造方法
    - 虚基类，由最终的派生类直接调用超级基类的构造函数
    - 一般继承：由直接基类构造超级基类，派生类中有两份超级基类对象

```c++
template<typename T>
class SealedT
{
	// 注意：不是 friend class T;
	friend T;

private:
	SealedT() {}
};

// 注意：是虚继承
class Demo2 :public virtual SealedT<Demo2>
{
};

class Test2 : public Demo2 
{
};

int main()
{
	Demo2*      pd2 = new Demo2;
	//  Test2*      pt2 = new Test2;        // 错误：不能间接构造虚基类SealedT
}
```

- 4.使用 虚继承 + 受保护的基类构造函数（这个不行）

```c++
#include <iostream>
using namespace std;

template <class T>
class Assistant
{
 private:
    friend  T;  //注意这里的友元，注意：不是 friend class T
    Assistant(){};
    ~Assistant(){};
};

class A : public virtual Assistant <A>
{
 public:
    A(){};
    ~A(){};
};

class B : public A
{
 public:
    B(){}; //B的构造函数需要直接调用A的构造函数，但其不是A的友元
    ~B(){};
};

int main(int argc, char* argv[])
{
    A a;       // 可以构造
    B b;       // 不能构造
    return 0;
}
```

[阻止类被继承（继承类被实例化）的几种常用办法](https://blog.twofei.com/672/)

[C++中如何防止类被继承](http://www.cnblogs.com/kingstarspe/archive/2013/06/06/virtualpublic.html)

## 8.声明为protected的基类构造函数

可以调用的地方：

- 派生类构造函数默认调用
- 派生类构造函数初始化列表（此时可以调用基类的非默认构造函数）
- 派生类默认构造函数函数体（此时只能调用基类的默认构造函数）
- 除此之外，还有以下情况：

```c++
class A {
protected:
	A()
	{
		cout << "A";
	}
	A(int)
	{
		cout << "A(int)";
	}
};

class B : public A {
public:
	B(){
		A a1(); //正确
		A a2(2); //错误
	}
	B(int){
		A a1(); //正确
		A a2(2); //错误
	}
};
```

## 9.面向对象的三大特征

封装、继承、多态

## 10.移动构造函数

//TODO

[移动构造-C++11](https://blog.csdn.net/zyq11223/article/details/48766515)

## 11.移动赋值运算符

//TODO

（与noexcept？何时合成）

## 12.阻止拷贝

- 1.新方案：可以通过将拷贝构造函数和拷贝赋值运算符定义为删除的函数（deleted function）来阻止拷贝。虽然声明了他们，但是不能以任何形式使用他们
  - 注意：析构函数不能定义为删除

```c++
struct NoCopy
{
    NoCopy() = default;    //使用合成的默认构造函数
    NoCopy(const NoCopy&) = delete;  //阻止拷贝
    NoCopy & operator = (const NoCopy&) = delete; //阻止赋值
    ~NoCopy() = default; //使用合成的析构函数
    //其他成员
};
```

- 2.在private区域声明但不实现
  - 好处：禁止友元函数和类成员函数也禁止了复制

```c++
class CPeople  
{  
private:  
    // 将复制相关的操作定义为私有  
    CPeople(CPeople&); // 只声明不实现  
	const CPeople& operator=(const CPeople& rhis); // 只声明不实现  
}; 
```

## 13.对象或对象的引用（指针）调用构造函数和析构函数

- 构造函数：不能通过对象或引用调用构造函数
- 析构函数：可以通过对象或引用调用析构函数

```c++
class A {
public:
	A() { cout << "A()"; }
	A(int) { cout << "A(int)"; }
	~A() { cout << "~A()"; }
};
int main(int argc, char* argv[])
{
	//正确
	A* a1 = new A();
	a1->~A();
	//正确
	A a2;
	a2.~A();
	//错误
	A a3;
	a3.A();
	//错误
	A* a4 = new A();
	a4->A();
	return 0;
}
```

## 14.删除的合成函数

//TODO

[删除的合成函数](https://github.com/arkingc/llc/blob/master/cpp/class/delete/README.md)（一般函数而言不想调用的话不定义就好）

## 15.派生类中的复制控制

[C++派生类的构造函数和复制控制](https://blog.csdn.net/zhy_cheng/article/details/8201250)

- 1.复制构造函数
  - 先调用基类的复制构造函数，初始化基类的成员，然后初始化本类的成员

```c++
Son::Son(const Son& ss):Father(ss)
{
	this->pcname=ss.pcname;
	this->phonename=ss.phonename;
}
```

- 2.赋值操作符

```c++
Son& Son::operator =(Son& ss)
{
    if(this==&ss) return *this;
	Father::operator=(ss);
	this->pcname=ss.pcname;
	this->phonename=ss.phonename;
	return *this;
}
```

- 需要显式调用基类的赋值操作符
- 如果是自赋值，则不作任何处理，直接返回 *this；如果不是自赋值，首先释放实例自身已有内存，然后再分配新的内存

## 16.类内定义常量

[在类中定义常量的几个做法](http://www.cnblogs.com/huhu0013/archive/2010/09/13/1824989.html)

- 1.const：注意，必须在构造函数的初始化列表中初始化，否则，构造函数会被删除

- 2.enum：`enum { size = 100} a;`
  - 使用enum不会占用对象中的存储空间的，枚举常量在编译的时候被全部求值
    - [C++ 中的 enum 结构在内存中是怎么存储的？](http://www.cnblogs.com/ysdu/p/4690090.html)
    - 关于占用内存的大小，enum类型本身是不占内存的，编译器直接替换。但是enum类型的变量肯定是占内存的

  ```
  enum EnumTest{a =-2,b = 5};
  绝对值最大的是5，需要3bits能够容纳下，但是因为有负值，需要增加一个符号位，于是需要4bits，取值范围是[-8,7]。
  C++规定枚举的大小只要能够容纳下3bits或者4bits（上面的例子）就行，至于分配1byte还是4bytes，是编译器决定的，但是C++标准有个限制:1<=sizeof(enum)<=sizeof(int)
  //使用
  class D {
  public:
  	enum { t = 1,h=2 } a;
  	enum { l = 1, j = 2 } b;
  };

  int main(int argc, char* argv[])
  {
  	D d;
  	d.a = d.h;
  	cout << d.a;
  	system("pause");
  	return 0;
  }
  ```

  - 缺点：假如定义一个非整型的常量该如何？enum无法完成此项操作,同时丧失了枚举本来的作用

- 3.static const，在类外初始化

## 17.struct和class的区别

[C/C++的class和struct的区别](https://blog.csdn.net/u010575592/article/details/52176662)

C的struct与C++的class的区别：

- struct只是作为一种复杂数据类型定义，不能用于面向对象编程。
- C++中的struct和class的区别：对于成员访问权限以及继承方式，class中默认的是private的，而struct中则是public的。class还可以用于表示模板类型，struct则不行。 

## 18.成员函数体内、成员函数的参数列表的名字解析时机

参考《C++对象模型》3.1 数据成员的绑定

**类成员的inline成员函数的数据成员绑定：** 

- 1.函数体中的变量值的解析工作，在类声明完成之后才会进行
- 2.函数的参数列表中名称的解析工作，在出现参数列表代码的时候就开始进行

```c++
typedef int length;
int _val = 10;

class Point3d
{
public:
    //length为int，被解析为global，即::length
    // _val被解析为Point3d::_val;
    void mumble( length val ) { _val = val; }

private:
    typedef float length;
    //length被解析为float，即Point3d::length
    length _val;
};
```

为了避免2.中的情况，可以使用“防御性程序风格”

- a.把所有数据成员放在class声明起头处，以确保正确的绑定

除了a.之外，还有别的“防御性程序风格”，但这对解决上面2.的问题没有帮助

- b.把所有的inline functions，不管大小，都放在class声明之外

## 19.virtual函数动态绑定，缺省参数值静态绑定 

[《Effective C++:条款37》](https://github.com/arkingc/note/blob/master/C++/EffectiveC++.md#%E6%9D%A1%E6%AC%BE37%E7%BB%9D%E4%B8%8D%E9%87%8D%E6%96%B0%E5%AE%9A%E4%B9%89%E7%BB%A7%E6%89%BF%E8%80%8C%E6%9D%A5%E7%9A%84%E7%BC%BA%E7%9C%81%E5%8F%82%E6%95%B0%E5%80%BC) 

绝不重新定义继承而来的缺省参数值。原因就在于，virtual函数是动态绑定，而缺省参数值却是静态绑定。所以你可能调用了一个派生类的virtual函数，但是使用到的缺省参数，却是基类的
但注意：对象调用时，是静态调用，使用的是派生类的缺省值

总的来说，就是根据调用者的静态类型来确认缺省值

```c++
class Shape{
public:
    enum ShapeColor {Red,Green,Blue};
    virtual void draw(ShapeColor color = Red) const = 0;
    ...
};

class Rectangle : public Shape {
public:
    virtual void draw(ShapeColor color = Green) const;
    ...
};

class Circle : public Shape {
public:
    virtual void draw(ShapeColor color) const;
    ...
};

Rectangle r;
Circle c;

r.draw();           //调用Rectangle::draw，静态类型为Rectangle，所以缺省参数为Shape::Green
//c.draw();         //调用Circle::draw，静态类型为Circle，没有缺省参数，因此错误，必须显示指定！

Shape *pr = &r;
Shape *pc = &c;

//以下为容易引起困惑的地方，函数与参数不一致
pr->draw();         //调用Rectangle::draw，但是静态类型为Shape，所以缺省参数Shape::Red
pc->draw();         //调用Shape::draw，但是静态类型为Shape，所以缺省参数Shape::Red
```

## 20.定义类内常量的方法

 [《Effective C++:条款2》](https://github.com/arkingc/note/blob/master/C++/EffectiveC++.md#%E6%9D%A1%E6%AC%BE02%E5%B0%BD%E9%87%8F%E4%BB%A5constenuminline%E6%9B%BF%E6%8D%A2define) 

- 1.static const：一般来说需要类外定义，除类整型的值。但有一些编译器不支持类内初始化
- 2.enum：类内定义
  - 对于enum的内存占用：
    - enum对象占用4字节
    - 声明不占用空间

```c++
class GamePlayer{
    enum {NumTurns = 5};
    int scores[NumTurns];   //这就没问题了
};
```

# 指针和引用

## 1.指针和引用的区别

- 1).指针保存的是所指对象的地址，引用是所指对象的别名，指针需要通过解引用间接访问，而引用是直接访问；
- 2).指针可以改变地址，从而改变所指的对象，而引用必须从一而终
- 3).引用在定义的时候必须初始化，而指针则不需要
- 4).指针有指向常量的指针和指针常量，而引用没有常量引用（引用本身就是“常量”的）
- 5).指针可以指向动态分配的内存，引用不可以
- 6).指针更灵活，用的好威力无比，用的不好处处是坑，而引用用起来则安全多了，但是比较死板

## 2.指针与数组的关系

不同之处

- 1).数组一旦声明，我们就不能再给数组名赋值，但是我们可以给指针赋值
- 2).用指针定义字符串时，指针所指的字符串会被理解为常量字符串，不能修改。数组定义的字符串则可以修改

	char str1[10] = "hello";
	const char* str2 = "hello";
- 3).用sizeof数组名可以计算数组变量的数组大小（函数形参除外），但指针指向数组时，只能计算出指针本身大小（单位：字节）

## 3.智能指针 

> 1).实现原理

使用智能指针缘由：

- a.内存泄漏
- b.空悬指针(野指针):指向“垃圾”内存
- c.申请失败时的异常处理：int *ptr = new(nothrow) int(0);，异常时返回nullptr

智能指针可以做到：

- a.智能指针能够帮助我们处理资源泄露问题
- b.它也能够帮我们处理空悬指针的问题
- c.它还能够帮我们处理比较隐晦的由异常造成的资源泄露

主要的智能指针类型：`shared_ptr`、`unique_ptr`、`weak_ptr`

**shared_ptr：** 

shared_ptr的目标就是，在其所指向的对象不再使用之后，自动释放与对象相关的资源（引用计数为0时）。

shared_ptr本身提供默认内存释放器，调用delete。当然我们也可以自己定义内存释放器。默认内存释放器并不能释放数组内存空间，而是要我们自己提供内存释放器。

```c++
shared_ptr<int> pJutta2(new int[10],
2         // deleter (a lambda function) 
3         [](int *p)
4         { 
5             delete[] p;
6         }
7     );

shared_ptr<int> p(new int[10], default_delete<int[]>());
```

实现中需要注意的是：

- 1.构造函数时，count=1
- 2.拷贝构造函数，count+=1
- 3.析构函数，count--，如果count为0，则释放内存
- 4.赋值操作符：右操作数的count先加1，左操作数的count再减1（这样的顺序是为了防止自赋值时出错），如果左操作数count为0，则释放左操作数原内存，并指向右操作数的内存空间

![](../../../pics/interview/language/C++/shared_ptr和unique_ptr都支持的操作.png)

**unique_ptr：** 

unique_ptr是一种在异常发生时可帮助避免资源泄露的智能指针。这个智能指针实现了独占式拥有概念，意味着它可确保一个对象和其相应资源同一时间只被一个指针拥有。一旦拥有者被销毁或变成空，或开始拥有另一个对象，先前拥有的那个对象就会被销毁，其任何相应资源也会被释放。

不支持普通的拷贝或赋值操作，但可以通过调用release或reset将指针的所有权从一个（非const）unique_ptr转义给另一个unique_ptr。

不能拷贝unique_ptr的规则有一个例外：我们可以拷贝或赋值一个将要被销毁的unique_ptr，最常见的例子是从函数返回一个unique_ptr。

```c
unique_ptr<int> ptr(new int(0));

//将所有权从p3转移给p2
p2.reset(p3.release());  //reset释放了p2原来指向的内存
```

![](../../../pics/interview/language/C++/unique_ptr操作.png)

**weak_ptr** 

- weak_ptr是一种不控制所指向对象生成期的智能指针
- weak_ptr指向由一个shared_ptr管理的对象
- 将一个weak_ptr绑定到一个shared_ptr不会改变shared_ptr的引用计数
- shared_ptr被销毁，即使weak_ptr指向对象，对象也还是会被释放
- 由于对象可能不存在，不能使用weak_ptr直接访问对象，必须调用lock
- lock检查weak_ptr所指向的对象是否存在，如果存在，lock返回一个指向共享对象的shared_ptr
- 好处：避免循环引用

[C++智能指针及其简单实现](http://www.cnblogs.com/xiehongfeng100/p/4645555.html)

![](../../../pics/interview/language/C++/weak_ptr.png)

> 2).如何改变引用计数

- 构造函数中计数初始化为1；
- 拷贝构造函数中计数值加1；
- 赋值运算符中，右边的对象引用计数加1，左边的对象引用计数减1；
- 析构函数中引用计数减1；
- 在赋值运算符和析构函数中，如果减1后为0，则调用delete释放对象

> 3).share_ptr循环引用

//TODO

```c
//share_ptr可能出现循环引用，从而导致内存泄露
class A
{
public:
    share_ptr<B> p;
};

class B
{
public:
    share_ptr<A> p;
}

int main()
{
    while(true)
    {
        share_prt<A> pa(new A()); //pa的引用计数初始化为1
        share_prt<B> pb(new B()); //pb的引用计数初始化为1
        pa->p = pb; //pb的引用计数变为2
        pb->p = pa; //pa的引用计数变为2
    }
    //假设pa先离开，引用计数减1变为1，不为0因此不会调用class A的析构函数，因此其成员p也不会被析构，pb的引用计数仍然为2；
    //同理pb离开的时候，引用计数也不能减到0
    return 0;
}

/*
** weak_ptr是一种弱引用指针，其存在不会影响引用计数，从而解决循环引用的问题
*/
```

---

# 表达式、变量

## 1.四种类型转换

C++四种类型转换：static_cast, dynamic_cast, const_cast, reinterpret_cast

使用类型转换，得到的是被转换对象的副本（Effective C++ P119）

`cast-name<type>(expression);`

> 1).static_cast

static_cast能进行基础类型之间的转换（用来强迫隐式转换），有如下几种用法：

- a.能隐式转换，包括：
  - 基本内置数据类型之间的转换：如把int转换成char，把int转换成enum。这种转换的安全性也要开发人员来保证
  - 派生类向基类（指针/对象）：进行上行转换（把子类的指针或引用转换成父类表示）是安全的
  - 把任何类型的指针转换成void类型指针
- b.基类向派生类的转换（指针/对象）
  - 基类需要实现虚函数（typeid），非虚继承
  - 由于没有动态类型检查，所以是不安全的
  - 是对象时需要隐式转换，如果不支持隐式转换，是无法进行的
- c.把void指针转换成目标类型的指针
  - 不安全

static_cast不能转换掉expression的const、volitale、或者__unaligned属性

> 2).const_cast

- a.该运算符用来修改类型的const或volatile属性。除了去掉const 或volatile修饰之外， type_id和expression得到的类型是一样的
- b.const_cast只能改变运算对象的底层const，即const_cast中的参数只能是指针或引用

```c
const char *pc;
char *p = const_cast<char *>(pc); //正确，但是通过p写值是未定义的行为
```

> 3).reinterpret_cast

- a.reinterpret_cast通常为运算对象的位模式提供较低层次上的重新解释
- b.它可以把一个指针转换成一个整数，也可以把一个整数转换成一个指针（先把一个指针转换成一个整数，在把该整数转换成原类型的指针，还可以得到原先的指针值）

```c
int *ip;
//以上pc所指的对象是一个int而非字符，如果把pc当普通字符指针使用就可能在允许时发生错误
char *pc = reinterpret_cast<char*>(ip);
//可能导致异常的运行时行为
string str(pc);
```

> 4).dynamic_cast

- a.只能用于继承体系中指针引用之间的转换
- b.可以是虚继承
- c.dynamic_cast 主要用在继承体系中的安全向下转型（基类指针转向派生类指针）
- d.它能安全地将指向基类的指针转型为指向派生类的指针或引用，并获知转型动作成功是否。转型失败会返回null（转型对象为指针时）或抛出异常（转型对象为引用时）
- e.dynamic_cast 会动用运行时信息（RTTI）来进行类型安全检查，因此 dynamic_cast 存在一定的效率损失
	 d.dynamic_cast 只有在基类带有虚函数的情况下才允许将基类转换为子类（因为要检查typeid，而动态类型的typeid是在虚函数表中的）	

> 旧式的强制类型转换

```c
type (expr);  //函数形式的强制类型转换
(type) expr;  //C语言风格的强制类型转换
```

- a.旧式的强制类型转换分别具有与const_cast、static_cast或reinterpret_cast相似的行为
- b.当我们在某处执行旧式的强制类型转换时，如果换成const_cast和static_cast也合法，则行为与对应的命名转换一致。如果替换后不合法，则旧式强制类型转换执行与reinterpret_cast类似的功能

[C++四大类型转换](https://blog.csdn.net/Robin__Chou/article/details/51957686)

## 2.前置++和后置++的区别

```c++
// 前置++，返回自增后的值，且返回的是一个左值
int& operator++(){
    *this += 1;
    return *this;
} 

// 后置++，返回自增前的值，且返回的是一个右值
const int operator++(int){
    int temp(*this);
    *this += 1;
    return temp;
}
```

## 3.如何比较浮点数大小？

两个浮点数比较只能通过相减并与预先设定的精度比较，记得要取绝对值！ 

相关函数：

- `extern float fabs(float x); `
- abs函数是针对整数的

```c++
if( fabs(f1-f2) < 预先指定的精度）
{
      ...
}
```

# 函数

## 1.内联函数与宏定义

内联函数的优点，与宏定义的区别

- 1).宏定义在预编译的时候就会进行宏替换
- 2).内联函数在编译阶段，在调用内联函数的地方进行替换，减少了函数的调用过程，但是使得编译文件变大。因此，内联函数适合简单函数，对于复杂函数，即使定义了内联编译器可能也不会按照内联的方式进行编译 
- 3).内联函数相比宏定义更安全，内联函数可以检查参数，而宏定义只是简单的文本替换。因此推荐使用内联函数，而不是宏定义
- 4).使用宏定义函数要特别注意给所有单元都加上括号，\#define MUL(a, b) a * b，这很危险，正确写法：\#define MUL(a, b) ((a) * (b))

## 2.函数重载

**普通函数重载**

如果函数名相同，在相同的作用域内，其参数类型、参数个数，参数顺序不同等能构成函数重载。

仅仅返回值类型不同，是不能构成函数重载的

**成员函数重载**

需要注意：重写、隐藏等问题

**继承中的重载**

[C++继承中重载、重写、重定义的区别](http://www.cnblogs.com/weizhixiang/articles/5760286.html)

**前提：**

- 重载overload：是函数名相同，参数列表不同，重载只是在类的内部存在（相同的作用域）。但是不能靠返回类型来判断。
- 重写override：也叫做覆盖。**子类重新定义父类中有相同名称和参数的虚函数。**函数特征相同。但是具体实现不同，主要是在继承关系中出现的 。
- 重定义 (redefining)也叫做隐藏:子类重新定义父类中有相同名称的**函数** ( 参数列表可以不同 ) 

**重写需要注意：**

- 被重写的函数不能是static的。必须是virtual的
- 重写函数必须有相同的类型，名称和参数列表
- 重写函数的访问修饰符可以不同。尽管virtual是private的，派生类中重写改写为public,protected也是可以的
- 返回值为指针或引用是，派生类返回的指针和引用类型可以是基类的返回的类型的派生类

**有以下总结：**

- 成员函数重载特征：
  - 相同的范围（在同一个类中），有例外：使用using时，下面详细分析
  - 函数名字相同
  - 参数不同
  - virtual关键字可有可无，但不能作为重载的依据（就是有virtual和无virtual不算重载的一种）
- 重写（覆盖)是指派生类函数覆盖基类函数，特征是：
  - 范围：分别位于基类和派生类中
  - 函数的名字相同
  - 参数相同
  - 基类函数必须有virtual关键字
- 重定义(隐藏)是指派生类的函数屏蔽了与其同名的基类函数，规则如下：
  - 范围：基类和派生类之间
  - 参数不相同，可虚可不虚：如果派生类的函数和基类的函数同名，但是参数不同，此时，不管有无virtual，基类的函数被隐藏（如果是虚函数，实际上是重载后隐藏了基类函数，如下面代码所示）
  - 参数相同，且非虚：如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有vitual关键字，此时，基类的函数被隐藏 

```c++
class A {
public:
	virtual int fun() {}
};
class B :public A {
public:
	int fun(int a) {}  //这是重载而不是重写,此时A::fun()被隐藏
};

int main(int argc, char* argv[])
{
	
	B b;
	b.fun(); //错误
	b.A::fun(); //正确
	system("pause");
	return 0;
}
```

**重载与using**

```c++
class A {
public:
	virtual int fun() {}
};
class B :public A {
public:
	using A::fun; //使用了using
	int fun(int a) {}  //这是重载而不是重写,此时A::fun()不会被隐藏，因为使用了using，且fun()为虚函数，fun(int)为非虚函数
};

int main(int argc, char* argv[])
{
	B b;
	b.fun();
	system("pause");
	return 0;
}
```

**重载与const**

情况一：

如果同时在类中，对于函数名相同的const函数和非const函数能够构成重载，它们被调用的时机为：如果定义的对象是常对象，则调用的是const成员函数，如果定义的对象是非常对象，则调用重载的非const成员函数

```c++
class A
{
	int func( void );
	int func( void ) const;
}
```

情况二：

参数是const。

- 1.顶层const，即指针是常指针时，不算重载

```c++
//不算重载
void fun(int* const i) {
	cout << "fun1";
}
void fun( int* i) {
	cout << "fun2";
}
```

- 2.底层const算重载

```c++
void fun( const int*  i) {
	cout << "fun1";
}
void fun( int* i) {
	cout << "fun2";
}
int main(int argc, char* argv[])
{
    const int * i1 = 0;
    fun(i1); //输出fun1
	int* i2 = 0;
	fun(i2); //输出fun2
	return 0;
}
```

## 3.非静态成员函数的调用

- 1.对于没有使用非静态成员的非静态成员函数，该类的空指针也可以调用
- 2.不能使用作用域运算符（`::`）调用，只能使用`->`（指针时），或`.`（对象时）

## 4.lambda

[C++ 11 Lambda表达式](http://www.cnblogs.com/DswCnblog/p/5629165.html)

好处：可以方便的定义和创建匿名函数

```c++
[capture list] (params list) mutable exception-> return type { function body }

省略版本：
//这种类型的表达式不能修改捕获列表中的值
[capture list] (params list) -> return type {function body}
//省略了返回值类型，1).有return语句，根据语句判断；2).没有return,返回类型void
[capture list] (params list) {function body}
//省略了参数列表
[capture list] {function body}

1. capture list：捕获外部变量列表
2. params list：形参列表
3. mutable指示符：用来说用是否可以修改捕获的变量
4. exception：异常设定
5. return type：返回类型
6. function body：函数体

[c,&d](int a, int b) mutable -> bool 
{ 
	c++;
	d++;
	return a < b; 
}
```

---

## 5.static函数与普通函数的区别

[c语言中static 函数和普通函数的区别](https://blog.csdn.net/junboyboy/article/details/17921763)

>  静态函数 

在函数的返回类型前加上关键字static，函数就被定义成为静态函数。

函数的定义和声明默认情况下是extern的，但静态函数只是在声明他的文件当中可见，不能被其他文件所用

定义静态函数的好处： 

- 其他文件中可以定义相同名字的函数，不会发生冲突 
- 静态函数不能被其他文件所用 

# 模板

## 1.模板特例化

模板特化分为全特化和偏特化，模板特化的目的就是对于某一种变量类型具有不同的实现，因此需要特化版本。例如，在STL里迭代器为了适应原生指针就将原生指针进行特化。

模板有：函数模板、类模板

> 1).全特化

通过全特化一个模板，可以对一个特定参数集合自定义当前模板，类模板和函数模板都可以全特化。 全特化的模板参数列表应当是空的，并且应当给出”模板实参”列表：

```c++
// 全特化类模板
template <>
class A<int, double>{
    int data1;
    double data2;
};

// 函数模板
template <>
int max(const int lhs, const int rhs){   
    return lhs > rhs ? lhs : rhs;
}
```

注意:类模板的全特化时在类名后给出了“模板实参”，但函数模板的函数名后没有给出”模板实参”。 这是因为编译器根据int max(const int, const int)的函数签名可以推导出来它是T max(const T, const T)的特化。

函数模板在函数形参为空时，不能省略“模板实参”，否则会产生歧义。

> 2).偏特化

偏特化也是为了给自定义一个参数集合的模板，但偏特化后的模板需要进一步的实例化才能形成确定的签名。 值得注意的是函数模板不允许偏特化。

```c
//类模板
template <class T1, class T2>
class A{
    T1 data1;
    T2 data2;
};

//偏特化
template <class T2>
class A<int, T2>{
    ...
};
```

如果函数模板想要偏特化，可以用重载函数实现

```c
//重载函数1
template <class T1, class T2>
void f(T1 t1, T2 t2){}

//重载函数2
template <class T2>
void f(int t1, T2 t2){}
```

[C++模板的偏特化与全特化](http://harttle.land/2015/10/03/cpp-template.html)

## 2.宏定义与内联函数的区别

- 1).处理时间不一样：宏定义在预处理阶段进行代码替换，内联函数在编译阶段插入代码
- 2).安全性不一样：宏没有类型检查，内联函数有类型检查

## 3.非类型模板参数

[C++基础——非类型模板参数](https://blog.csdn.net/lanchunhui/article/details/49634077#非类型类模板参数)

它可以是常整数（包括enum枚举类型）或者指向外部链接对象的指针

```c++
//非类型类模板参数，例如array<int,10>
template<typename T, int MAXSIZE>
class Stack
{
}
//非类型函数模板参数
template<typename T, int VAL>
T addValue(const T& x)
{
    return x + VAL;
}
```

# 容器

## 1.array与内置数组的区别

- array是C++11中新提出来的容器类型，几乎等于（内置数组+容器特性）

- 内置数组相比

  - 更容易使用
  - 更加安全
  - 继承了数组最基本的特性
  - 也融入了很多容器操作


  - 拷贝和复制：内置数组只能遍历的形式拷贝，array可以使用拷贝和赋值的形式进行初始化
    - `array<int,10> copy=ial1;//只要保证两者的类型一致就可以（包括元素类型和大小）`

- array特点：

  - 固定大小的容器类型
  - 定义的时候就要声明大小和类型
  - 可以迭代器访问
  - 可以使用swap()来交换两个array

## 2.map和unordered_map的优劣

> 实现原理

map： map内部实现了一个红黑树（红黑树是非严格平衡二叉搜索树，而AVL是严格平衡二叉搜索树），红黑树具有自动排序的功能，因此map内部的所有元素都是有序的，红黑树的每一个节点都代表着map的一个元素。因此，对于map进行的查找，删除，添加等一系列的操作都相当于是对红黑树进行的操作。map中的元素是按照二叉搜索树（又名二叉查找树、二叉排序树，特点就是左子树上所有节点的键值都小于根节点的键值，右子树所有节点的键值都大于根节点的键值）存储的，使用中序遍历可将键值按照从小到大遍历出来。
unordered_map: unordered_map内部实现了一个哈希表（也叫散列表，通过把关键码值映射到Hash表中一个位置来访问记录，查找的时间复杂度可达到O(1)，其在海量数据处理中有着广泛应用）。因此，其元素的排列顺序是无序的

> 优缺点以及适用处

**map:**

[map和unordered_map的差别和使用](https://blog.csdn.net/BillCYJ/article/details/78985895)

- 优点：
  - 有序性，这是map结构最大的优点，其元素的有序性在很多应用中都会简化很多的操作 
  - 红黑树，内部实现一个红黑书使得map的很多操作在`lgn`的时间复杂度下就可以实现，因此效率非常的高 
- 缺点：空间占用率高，因为map内部实现了红黑树，虽然提高了运行效率，但是因为每一个节点都需要额外保存父节点、孩子节点和红/黑性质，使得每一个节点都占用大量的空间 
- 适用处：对于那些有顺序要求的问题，用map会更高效一些 

**unordered_map：** 

- 优点： 因为内部实现了哈希表，因此其查找速度非常的快 
- 缺点： 哈希表的建立比较耗费时间 
- 适用处：对于查找问题，`unordered_map`会更加高效一些，因此遇到查找问题，常会考虑一下用`unordered_map` 

> 对于自定义类型实现时的要求

[unordered_map 与 map 的对比](https://www.cnblogs.com/NeilZhang/p/5724996.html)

unordered_map和map类似，都是存储的key-value的值，可以通过key快速索引到value。不同的是unordered_map不会根据key的大小进行排序，

存储时是根据key的hash值判断元素是否相同，即unordered_map内部元素是无序的，而map中的元素是按照二叉搜索树存储，进行中序遍历会得到有序遍历。

所以**使用时map的key需要定义operator<。**而unordered_map需要定义**hash_value函数并且重载operator==。**但是很多系统内置的数据类型都自带这些，

那么如果是自定义类型，那么就需要自己重载operator<或者hash_value()了。

结论：如果需要内部元素自动排序，使用map，不需要排序使用unordered_map

> 总结

1. 内存占有率的问题就转化成红黑树 VS hash表 , 还是unorder_map占用的内存要高。
2. 但是unordered_map执行效率要比map高很多
3. 对于unordered_map或unordered_set容器，其遍历顺序与创建该容器时输入的顺序不一定相同，因为遍历是按照哈希表从前往后依次遍历的

#其他

## 1.C++与JAVA的对比

> 1).C/C++与JAVA的区别

> 2).C/C++较Java有性能优势，为什么

## 2.C++函数调用的压栈过程

[「十九」《C和指针》笔记](https://guanjunjian.github.io/2017/01/09/study-pointers-on-c-summary/#第18章-运行时环境)

## 3.sizeof和strlen的区别？（运算符与函数、计算的对象、编译时运行时）

[C++中sizeof与strlen的区别](https://blog.csdn.net/u012441543/article/details/45848913)

[C++ sizeof不完全总结](https://blog.csdn.net/u012441543/article/details/45830575)

- 1.strlen是函数,strlen(char*)函数求的是字符串的实际长度，直到遇到第一个'\0'，然后就返回计数值，且不包括'\0'，函数的返回值值在运行时确定。参数是指针或字符数组，当数组名作为参数传入时，实际上数组就退化成指针了
- 2.sizeof是操作符，不是函数，返回的是变量声明后所占的内存数，不是实际长度。其值在编译时即计算好了，参数可以是数组、指针、类型、对象、函数等。当计算数组的size时，数组不会退化成指针（数组是形参时，会退化为指针）

```c++
int main()  
{  
    char arr[20]="hello world!";   
    cout<<"strlen(arr) : "<<strlen(arr)<<endl;  //返回12，一共有12个字符，不包括'\0'  
    cout<<"sizeof(arr) : "<<sizeof(arr)<<endl;  //返回20，因为系统给数组分配了20个字节  
  
  
    cout<<"strlen(\"hello\") : "<<strlen("hello")<<endl;  //返回字符串长度5  
    cout<<"sizeof(\"hello\") : "<<sizeof("hello")<<endl;  //返回6,实际的存储空间还包含最后面的'\0'  
  
    const char *p="hello";  
    cout<<"strlen(p) : "<<strlen(p)<<endl;  //返回字符串长度5  
    cout<<"sizeof(p) : "<<sizeof(p)<<endl;  //返回指针p的内存空间大小4个字节，即sizeof(char*)
    cout<<"sizeof(p) : "<<sizeof(*p)<<endl; //返回长度1，即第一个字符的长度，即sizeof(char)
}  
```

## 4.C++是不是类型安全的？

类型安全：类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域

C++不是类型安全的：

- 两个不同类型指针可以强制转换
- 以把0当成bool类型的false，也可以当做int中的数字0

## 5.gcc和g++的区别？

- gcc代表GUN Compiler Collection，是一堆编译器的集合，包括g++？

## 6.C和C++的区别

[C和C++的特点与区别](https://blog.csdn.net/youwotianya/article/details/80714910)

>C语言特点

1.作为一种面向过程的结构化语言，易于调试和维护；

2.表现能力和处理能力极强，可以直接访问内存的物理地址；

3.C语言实现了对硬件的编程操作，也适合于应用软件的开发；

4.C语言还具有效率高，可移植性强等特点

> C++语言特点

1.在C语言的基础上进行扩充和完善，使C++兼容了C语言的面向过程特点，又成为了一种面向对象的程序设计语言；

2.可以使用抽象数据类型进行基于对象的编程；（封装）

3.可以使用多继承、多态进行面向对象的编程；（继承、多态）

4.可以担负起以模版为特征的泛型化编程。（多态）

> 总结

C++与C语言的本质差别：在于C++是面向对象的，而C语言是面向过程的。或者说C++是在C语言的基础上增加了面向对象程序设

计的新内容，是对C语言的一次更重要的改革，使得C++成为软件开发的重要工具。


