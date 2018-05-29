STL源码剖析 笔记

<!-- GFM-TOC -->
* [第1章 STL概论与版本简介](#第1章-stl概论与版本简介)
    * [1.2 STL 六大组件 功能与运用](#12-stl-六大组件-功能与运用)
    * [1.3 GNU 源代码开放精神](#13-gnu-源代码开放精神)
    * [1.8 SGI STL 实现版本](#18-sgi-stl-实现版本)
        * [1.8.1 GNU C++ headers 文件分布](#181-gnu-c++-headers-文件分布)
        * [1.8.2 SGI STL 文件分布与简介](#182-sgi-stl-文件分布与简介)
        * [1.8.3 SGI STL 的编译器组态设置](#183-sgi-stl-的编译器组态设置)
    * [1.9 可能令你困惑的 C++ 语法](#19-可能令你困惑的-c++-语法)
* [第2章 空间配置器 allocator](#第2章-空间配置器-allocator)
    * [2.1 空间配置器的标准接口](#21-空间配置器的标准接口)
    * [2.2 具备次配置力（sub-allocation）的SGI空间配置器](#22-具备次配置力sub-allocation的sgi空间配置器)
        * [2.2.1 SGI 标准的空间配置器 allocator](#221-sgi-标准的空间配置器-allocator)
        * [2.2.2 SGI 特殊的空间配置器 alloc](#222-sgi-特殊的空间配置器-alloc)
        * [2.2.3 构造和析构基本工具：construct()和destroy()](#223-构造和析构基本工具construct和destroy)
        * [2.2.4 空间的配置与释放 alloc](#224-空间的配置与释放-alloc)
        * [2.2.5 第一级配置器 __malloc_alloc_template 剖析](#225-第一级配置器-__malloc_alloc_template-剖析)
        * [2.2.6 第二级配置器 __default_alloc_template 剖析](#226-第二级配置器-__default_alloc_template-剖析)
        * [2.2.7 空间配置函数 allocate()](#227-空间配置函数-allocate)
        * [2.2.8 空间释放函数 deallocate()](#228-空间释放函数-deallocate)
        * [2.2.9 重新填充 refill()](#229-重新填充-refill)
        * [2.2.10 内存池 chunk_alloc()](#2210-内存池-chunk_alloc)
    * [2.3 内存基本处理工具](#23-内存基本处理工具)
        * [2.3.1 uninitialized_copy](#231-uninitialized_copy)
        * [2.3.2 uninitialized_fill](#232-uninitialized_fill)
        * [2.3.3 uninitialized_fill_n](#233-uninitialized_fill_n)
* [第3章 迭代器概念与traits编程技法](#第3章-迭代器概念与traits编程技法)
<!-- GFM-TOC -->

# 第1章 STL概论与版本简介

## 1.2 STL 六大组件 功能与运用

STL提供**六大组件**，彼此可以组合套用： 

- 1.**容器**（containers）：各种数据结构，如：vector、list、deque、set、map。用来存放数据。从实现的角度来看，STL容器是一种**class template** 
- 2.**算法**（algorithms）：各种常用算法，如：sort、search、copy、erase。从实现的角度来看，STL算法是一种 **function template**
- 3.**迭代器**（iterators）：容器与算法之间的胶合剂，是所谓的“泛型指针”。共有五种类型，以及其他衍生变化。从实现的角度来看，迭代器是一种将 `operator*`、`operator->`、`operator++`、`operator- -` 等指针相关操作进行重载的**class template**。所有STL容器都有自己专属的迭代器，只有容器本身才知道如何遍历自己的元素。原生指针(native pointer，就是普通指针)也是一种迭代器
- 4.**仿函数**（functors）：行为类似函数，可作为算法的某种策略（policy）。从实现的角度来看，仿函数是一种重载了operator()的class或**class template**。一般的函数指针也可视为狭义的仿函数
- 5.**配接器**（adapters）：一种用来修饰容器、仿函数、迭代器接口的东西。例如：STL提供的queue 和 stack，虽然看似容器，但其实只能算是一种容器配接器，因为它们的底部完全借助deque，所有操作都由底层的deque供应。
  - 改变 functors接口者，称为function adapter
  - 改变 container 接口者，称为container adapter
  - 改变iterator接口者，称为iterator adapter
- 6.**配置器**（allocators）：负责空间配置与管理。从实现的角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的**class template**

![](../../pics/language/STL源码剖析/img-1-1-STL六大组件的交互关系.png)

## 1.3 GNU 源代码开放精神

**GNU**(音译为“革奴”)，代码**G**UN is **N**ot **U**nix。GNU计划中，早期最著名的软件包括Emacs和GCC，晚期最著名的是Linux操作系统

**GNU以所谓的GPL(General Public License，广泛开放授权)来保护(或说控制)其成员**：使用者可以自由阅读与修改GPL软件的源码，但如果使用者要传播借助GPL软件而完成的软件，必须也同意GPL规范。这种精神主要是强迫人们分享并回馈他们对GPL软件的改善。得之于人，舍于人 

## 1.8 SGI STL 实现版本

**SGI STL实现版本**

- 继承自HP版本，所以每一个头文件都有HP的版本说明 
- 此外还加上SGI的公司版权声明 
- 不属于GNU GPL范畴，但属于open source范畴 
- **被GCC采用**（GCC对C++语言特性支持很好，连带给予了SGI STL正面影响） 
- 可读性很高 
- 为了具有高度移植性，考虑了不同编译器的不同编译能力 

### 1.8.1 GNU C++ headers 文件分布

**版本**：[cygnus c++ 2.91.57 for windows](../../source/STL/cygwin-b20.1-full2.zip)

在[[g++/](../../source/STL/g++)]下有128个文件，在[[g++/std](../../source/STL/g++/std)]下有8个文件

### 1.8.2 SGI STL 文件分布与简介

上一小节所呈现的众多头文件中，概略可分为五组：

- 1.**C++标准规范下的C头文件(无扩展名)**：cstdio，cstdlib，cstring，...
- 2.**C++标准程序库中不属于STL范畴者**：stream，string，...
- 3.**STL标准头文件(无扩展名)**：vector，deque，list，map，algorithm，functional，...
- 4.**C++标准定案前，HP所规范的STL头文件**：vector.h，deque.h，list.h，map.h，algo.h，functional.h，...
- 5.**SGI STL内部文件**(**STL真正实现与此**)：stl_vector.h，stl_deque.h，stl_list.h，stl_map.h，stl_algo.h，stl_functional，...

前两组不在本书讨论范围内

### 1.8.3 SGI STL 的编译器组态设置

不同的编译器**对C++语言的支持程度**不尽相同。作为一个希望具备广泛移植能力的程序库，SGI STL准备了一个**环境组态文件**[<stl_config.h>](../source/STL/g++/stl_config.h)，其中定义了许多常量，标示某些组态的成立与否，所有STL头文件都会直接或间接包含这个组态文件，并以条件式写法，让预处理器根据各个常量决定取舍哪一段程序代码，例如： 

![](../../pics/language/STL源码剖析/img-1-8-3-组态例子.png)

<stl_config.h>文件起始处有一份常量定义说明，然后针对各家不同的编译器以及可能的不同版本，给予常量设定

## 1.9 可能令你困惑的 C++ 语法

>  1.9.1 stl_config.h 中的各种组态

- 组态3：**__STL_STATIC_TEMPLATE_MEMBER_BUG**
  - 测试在class template中拥有static data members 
- 组态5：**__STL_CLASS_PARTIAL_SPECIALIZATION**
  - 测试class template partial specialization（类模板偏特化）——在class template的一般化设计之外，特别针对某些template参数做特殊设计  
- 组态6：**__STL_FUNCTION_TMPL_PARTIAL_ORDER**
  - 测试函数模板的偏特化
- 组态7：**__STL_EXPLICIT_FUNCTION_TMPL_ARGS**
  - 整个SGI STL内都没有用到这一常量定义
- 组态8：**__STL_MEMBER_TEMPLATES**
  - 测试 class template之内可否再有 template（members） 
- 组态10：**__STL_LIMITED_DEFAULT_TEMPLATES**
  - 测试template 参数可否根据前一个template参数而设定默认值 
- 组态11：**__STL_NON_TYPE_TMPL_PARAM_BUG**
  - 测试 class template 可否拥有 non-type template（非类型模板参数） 参数
- 组态：**__STL_EXPLICIT_FUNCTION_TMPL_ARGS**（bound friend templates）

```c++
 * ifdef __STL_EXPLICIT_FUNCTION_TMPL_ARGS
 * 	define __STL_NULL_TMPL_ARGS <>
 * else
 * 	define __STL_NULL_TMPL_ARGS
 * endif
 
 这个组态常量常常出现在类似这样的场合（class template的friend函数声明）：
 * // in <stl_stack.h>
 * template <class T,class Sequence = deque<T> >
 * class stack{
 * 	friend bool operator== __STL_NULL_TMPL_ARGS (const stack &,const stack &);
 * 	friend bool operator< __STL_NULL_TMPL_ARGS (const stack &,const stack &);
 * };
 展开后就变成了：
 * template <class T,class Sequence = deque<T> >
 * class stack{
 * 	friend bool operator== <> (const stack &,const stack &);
 * 	friend bool operator< <> (const stack &,const stack &);
 * };
```

- 组态：**__STL_TEMPLATE_NULL**（class template explicit specialization）

```c++
 * ifdef __STL_CLASS_PARTIAL_SPECIALIZATION
 * 	define __STL_TEMPLATE_NULL template<>
 * else
 * 	define __STL_TEMPLATE_NULL
 * endif
 
 这个组态常量常常出现在类似这样的场合：
 * // in <type_traits.h>
 * template <class type> struct __type_traits { ...};
 * __STL_TEMPLATE_NULL struct __type_traits<char> { ...};
 *
 * // in <stl_hash_fun.h>
 * template <class key> struct hash { ...};
 * __STL_TEMPLATE_NULL struct hash<char> { ...};
 * __STL_TEMPLATE_NULL struct hash<unsigned char> { ...};
 展开后就变成了：
 * template <class type> struct __type_traits { ...};
 * template<> struct __type_traits<char> { ...};
 *
 * template <class key> struct hash { ...};
 * template<> struct hash<char> { ...};
 * template<> struct hash<unsigned char> { ...};
```

>  1.9.2 临时对象的产生与运用

有时候刻意制造一些临时对象，刻意使程序干净清爽

刻意制造临时对象的**方法**是：在类型别名之后直接加上一堆小括号，并可指定初值，**例如**：`Shape(5,3)`或`int(8)`。其**意义**相当于调用相应的构造函数且不指定对象名称

STL最常将此技巧用于**仿函数**与**算法**的搭配上，例如：

```c++
template <typename T>
class print
{
public:
    void operator()(const T& elem)
    { cout << elem << ' '; }
};
//print<int>()是一个临时对象，不是一个函数调用操作
//这个对象被传入for_each中起作用，当for_each结束时
//这个临时对象生命周期结束
for_each(iv.begin(), iv.end(), print<int>());
```

>  1.9.3 静态常量整数成员在 class 内部直接初始化

如果类内含const static 整型数据成员，根据C++标准规定，我们可以在类内直接给予初值。所谓“整型”，不单只是int

```c++
template<typename T>
class testClass
{
public:
    static const int _datai = 5;
    static const long _datal = 3L;
    static const char _datac = 'c';
};
```

>  1.9.4 自增/自减/解引用 操作符

**自增（++）/解引用（*）**操作符在迭代器的实现上占有非常重要的地位，因为任何一个迭代器**必须**实现前进和取址功能

**有些**迭代器有双向移动功能，因此还需要实现**自减（--）**操作符

>  1.9.5 前闭合后开区间表示法 [)

一对迭代器所标示的是个所谓的前闭后开区间，以[first, last)表示。整个实际范围从first开始，直到last-1

> 1.9.6 function call 操作符（operator()）

许多STL算法都提供了两个版本

- 1.用于一般情况（例如排序时以递增方式排序）
- 2.用于特殊情况（例如排序时由使用者指定以何种特殊关系进行排序）

第2种情况中，需要用户指定某个条件或某个策略，而条件或策略的背后由一整组操作构成，便需要某种特殊的东西来代表这“一整组操作”，有两种方法：

- 1.**函数指针**。缺点：
  - 无法持有自己的状态（局部状态）
  - 无法达到组件技术上的可适配性（无法再将某些修饰条件加诸于其上而改变其状态）
- 2.**仿函数**：使用起来像函数一样的东西。如你针对某个类进程operator()重载，该类就称为一个仿函数

```c++
template <class T>
struct plus
{
	T operator()(const T& x, const T& y) const { return x + y; }
};

int main()
{
    //第1对括号产生仿函数的临时对象
    //第2对括号，以该临时对象，调取其operator()函数
    cout<< plus<int>()(43,50) << endl;
}
```

# 第2章 空间配置器 allocator

不称作”内存分配器“，是因为分配的空间不一定是内存，可以是磁盘或其它辅助存储介质。可以实现一个获取磁盘空间的allocator。不过这里介绍的空间分配器获取的空间是内存 

## 2.1 空间配置器的标准接口

```c++
allocator::value_type
allocator::pointer
allocator::const_pointer
allocator::reference
allocator::const_reference
allocator::size_type
allocator::difference_type
//rebind是一个嵌套的class template，class rebind<U> 拥有唯一成员other,是一个typedef，代表allocator<U>
allocator::rebind
//构造函数
allocator::allocator()
//拷贝构造函数
allocator::allocator(const allocator&) 
//泛化的拷贝构造函数
template <class U> allocator::allocator(const allocator<U>&)
//析构函数
allocator::~allocator()
//返回某个对象的地址，算式 a.address(x) 等同于&x
pointer allocator::address(reference x) const 
//返回某个const对象的地址，算式 a.address(x) 等同于&x
const_pointer allocator::address(const_reference x) const
//配置空间，足以容纳n个T对象
//第2个参数是提示，实现上可能会利用它来增进区域性，或完全忽略
pointer allocator::allocate(size_type n,const void* = 0)
//归还之前配置的空间
void allocator::deallocate(pointer p,size_type n)
//返回可成功配置的最大量
size_type allocator::max_size() const
//根据x（调用p所指向类型的构造函数），在p指向的地址构造一个T对象。相当于new((void*)p) T(x)
void allocator::construct(pointer p,const T& x)
//等同于p->~T()，析构地址p的对象
void allocator::destroy(pointer p)
```

**但**这些结构：

**完全无法应用于SGI STL**，因为SGI STL在这个项目上根本就脱离了STL标准规格，使用一个专属的、拥有次层配置能力的、效率优越的特殊分配器。事实上SGI STL仍然提供了一个标准的分配器接口，只是把它做了一层隐藏，这个标准接口的分配器名为simple_alloc 

## 2.2 具备次配置力（sub-allocation）的SGI空间配置器

SGI STL的配置器与众不同：

- **名称**是alloc（标准为allocator）
- 不接受任何参数

在程序中显示采用SGI配置器

```c++
vector<int, std::alloc> iv;
```

SGI STL未符合标准，但通常不会给我们带来困扰，因为通常使用默认的空间配置器，SGI STL的每个容器已经指定其缺省的空间配置器为alloc，如vector的声明：

```c++
template<class T, class Alloc = alloc> //缺省使用alloc为配置器
class vector { ... };
```

### 2.2.1 SGI 标准的空间配置器 allocator

虽然SGI也定义有一个符合部分标准、名为[allocator](../../source/STL/g++/defalloc.h)（defalloc.h）的配置器，但SGI自己从未用过它，也不建议我们使用。主要原因是效率不佳，只把C++的::operator new和::operator delete做一层薄薄的包装而已 

### 2.2.2 SGI 特殊的空间配置器 alloc

STL allocator将**配置内存**和**构造对象**分开，将**对象析构**和**释放内存**分开

- **alloc::allocate()**负责内存配置，**alloc::deallocate()**负责释放内存
- **::construct()**负责构造对象，**::destroy()**负责对象析构

STL标准规定分配器定义于`<memory>`中，SGI`<memory>`内含两个文件（负责分离的2阶段操作 ）：

[<memory>](../../source/STL/g++/memory)

```c++
#include <stl_alloc.h>      //负责内存空间的配置与释放
#include <stl_construct.h>  //负责对象内容的构造与析构
```

![](../../pics/language/STL源码剖析/img-2-STL配置器定义图.png)

### 2.2.3 构造和析构基本工具：construct()和destroy()

[<stl_construct.h>](../../source/STL/g++/stl_construct.h)

![](../../pics/language/STL源码剖析/img-2-1-construct和destroy示意.png)

**destroy有两个版本**：

- 1.接收一个指针，准备将该指针所指向的析构掉
- 2.接收first和last两个迭代器，准备将[first,last)范围内的所有对象析构掉
  - 有用的析构函数：遍历析构
  - 无用的析构函数：什么也不做

**注意**：STL规定分配器必须拥有名为construct()和destroy()的两个成员函数，然而SGI特殊的空间分配器std::alloc并未遵守这一规则，所以实际上这部分属于STL allocator，但不属于std::alloc。即，SGI特殊的空间分配器std::alloc不包含”2.2.3  造和析构基本工具：construct()和destroy()“，只包含”2.2.4 空间的配置与释放，std::alloc“

### 2.2.4 空间的配置与释放 alloc

空间配置和空间释放由[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)负责

SGI对内存分配与释放的设计哲学如下： 

- 向system heap申请空间
- 考虑多线程状态（本章的讨论暂不考虑多线程）
- 考虑内存不足时的应变措施
- 考虑过多“小型区块”可能造成的内存碎片问题（**SGI设计了双层级分配器**）

C++的内存分配基本操作是::operator new(),内存释放基本操作是::operator delete()。这两个全局函数相当于C的malloc()和free()函数。SGI正是以malloc和free()完成内存的分配与释放

**双层级配置器**：

考虑到小型区块所可能造成的内存碎片问题，SGI设计了双层级分配器： 

- **第一级分配器**
  -  直接使用malloc()和free()
- **第二级分配器**：视情况采用不同的策略
  -  当分配区块超过128bytes时，视为“足够大”，调用第一级分配器
  - 当分配区块小于128bytes时，视为“过小”，为了降低额外负担，采用复杂的memory pool整理方式，不再求助于第一级分配器

![](../../pics/language/STL源码剖析/img-2-2a-第一级配置器与第二级配置器.png)

整个设计究竟只开放第一级配置器，或是同时开放第二级配置器，取决于**__USE_MALLOC**是否被定义（定义则只开放第一级配置器）

无论alloc被定义为第一级或第二级分配器，SGI还为它再包装一个接口，使分配器的接口能够符合STL规格： 

```c++
//Alloc传入的是配置器的类型（一级或二级）
template<class T, class Alloc>
class simple_alloc {

public:
    static T *allocate(size_t n)
                { return 0 == n? 0 : (T*) Alloc::allocate(n * sizeof (T)); }
    static T *allocate(void)
                { return (T*) Alloc::allocate(sizeof (T)); }
    static void deallocate(T *p, size_t n)
                { if (0 != n) Alloc::deallocate(p, n * sizeof (T)); }
    static void deallocate(T *p)
                { Alloc::deallocate(p, sizeof (T)); }
};
```

内部四个成员函数其实都是单纯的转调用，调用传递给配置器（一级或二级）的成员函数，**这个接口使分配器的分配单位从bytes转为个别元素的大小**。SGI STL容器全部使用这个simple_alloc接口，如图：

![](../../pics/language/STL源码剖析/img-2-2b-第一级配置器与第二级配置器的包装接口和运用方式.png)

### 2.2.5 第一级配置器 __malloc_alloc_template 剖析

**源码分析**：

第一级分配器__malloc_alloc_template定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中： 

[__malloc_alloc_template的实现](STL/__malloc_alloc_template.md)

- 1.第一级配置器以malloc()、free()、realloc()等C函数执行实际的内存分配、释放、重分配操作 
- 2.实现出类似C++ new-handler的机制
  - 由于SGI以malloc而非::operator new来配置内存，因此SGI不能直接使用C++的set_new_handler() ，必须仿真一个类似的函数
  - **C++ new handler机制**：可以要求系统在内存分配需求无法被满足时，调用一个你所指定的函数。换句话说，一旦::operator new无法完成任务，在丢出std::bad_alloc异常状态之前，会先调用由客户指定的处理例程，该处理例程通常即被称为new-handler
- 3.oom_malloc和oom_realloc：循环调用“内存不足处理例程”，期望在某次调用之后，获得足够的内存而圆满完成任务。如果“内存不足处理例程”并未被客端设定，这两个函数便不客气地调用__THROW_BAD_ALLOC丢出**bad_alloc**异常信息，或利用exit(1)中止程序

### 2.2.6 第二级配置器 __default_alloc_template 剖析

第二级分配器多了一些机制，避免太多小额区块造成内存的碎片，小额区块带来以下问题： 

- 1.产生内存碎片 
- 2.配置时的额外负担：额外负担是一些区块信息，用以管理内存。区块越小，额外负担所占的比例就越大，越显浪费 

![](../../pics/language/STL源码剖析/ima-2-3-额外负担.png)

SGI第二级配置器的**做法**：

- 1.**大区块**：区块大于128bytes时
  - 移交第一级配置器处理
- 2.**小额区块**：当区块小于等于128bytes时 
  - 以**内存池管理(也称为次层分配)** 
    - 每次分配一大块内存，并维护对应的自由链表(free-list)，下次若载有相同大小的内存需求，就直接从free-list中拨出。如果客户释放小额区块，就由分配器回收到free-list中
    -  **维护有16个free-list**，各自管理大小分别为8，16，24，32，40，48，56，64，72，80，88，96，104，112，120，128bytes的小额区块 
    - SGI第二级分配器会主动将任何小额区块的内存需求量上调至8的倍数（例如客户要求30bytes，就自动调整为32bytes）

**free-list节点结构**：

```c++
union obj{
    union obj * free_list_link; //系统视角
    char client_data[1];        //用户视角
}
```

为了维护链表，每个节点需要额外的指针（指向下一个节点），解决办法：使用union

- 从obj的第一字段观之，obj可被视为一个指针，指向相同形式的另一个obj
- 从obj的第二字段观之，obj可视为一个指针，指向实际区块

![](../../pics/language/STL源码剖析/img-2-4-自由链表的实现技巧.png)

**源码分析**：

第二级分配器__default_alloc_template定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中：

[__default_alloc_template的实现](STL/__default_alloc_template.md)

### 2.2.7 空间配置函数 allocate()

函数**做法**：

- 1.若区块大于128bytes，就调用第一级分配器 
- 2.若区块小于128bytes，检查对应的free-list 
  - 若free-list之内有可用的区块，则直接使用 
  - 若free-list之内没有可用区块，将区块大小调至8倍数边界，调用refill()，准备为free-list重新填充空间 

**源码分析**：

allocate()定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中

[allocate()的实现](STL/allocate().md)

![](../../pics/language/STL源码剖析/img-2-5-区块free-list拔出.png)

### 2.2.8 空间释放函数 deallocate()

函数**做法**：

- 1.若区块大于128bytes，就调用第一级分配器 
- 2.若区块小于128bytes，找出对应的free-list，将区块回收 

**源码分析**：

deallocate()定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中

[deallocate()的实现](STL/deallocate().md)

![](../../pics/language/STL源码剖析/img-2-6-区块回收-纳入free-list.png)

### 2.2.9 重新填充 refill()

allocate()发现free list中没有可用块区了时，就调用refill()，准备为free list重新填充空间。新的空间将取自内存池（由chunk_alloc()完成）。缺省取得20个新节点（新区块），内存池空间不足时获得的节点数可能小于20

**源码分析**：

refill()定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中

[refill()的实现](STL/refill().md)

### 2.2.10 内存池 chunk_alloc()

该函数的工作：从内存池中取出空间给free list使用

**函数思路**：

chunk_alloc()函数从内存池申请空间，根据`end_free-start_free`判断内存池中剩余的空间

- 如果剩余空间充足 
  - 直接调出20个区块返回给free-list
- 如果剩余空间不足以提供20个区块，但足够供应至少1个区块
  - 拨出这不足20个区块的空间（能分配多少区块，就分配多少区块）
- 如果剩余空间连一个区块都无法供应
  - 试着让内存池中的残余零头还有利用价值，分配给适当的free-list
  - 利用malloc()从heap中分配内存（大小为需求量的2倍，加上一个随着分配次数增加而越来越大的附加量），为内存池注入新的可用空间
    - 如果malloc()获取失败，chunk_alloc()就四处寻找有无”尚有未用且区块足够大“的free-list（大小为[size,128]的free-list）。找到了就挖出一块交出。递归调用自身
    - 如果上一步仍为失败，那么就调用第一级分配器，第一级分配器有out-of-memory处理机制，或许有机会释放其它的内存拿来此处使用。如果可以，就成功，否则抛出bad_alloc异常 
  - 如果上面的步骤找到一定的内存空间，则递归调用自身

**源码分析**：

chunk_alloc()定义在头文件[<stl_alloc.h>](../../source/STL/g++/stl_alloc.h)中

[chunk_alloc()的实现](STL/chunk_alloc().md)

**例子**：

- 1.一开始，客端调用chunk_alloc(32,20)，于是malloc()分配40个32bytes区块，其中第1个交出，另19个交给free-list[3]维护，余20个留给内存池 
- 2.接下来客户调用chunk_alloc(64,20)，此时free_list[7]空空如也，必须向内存池申请。内存池只能供应(32*20)/64=10个64bytes区块（步骤1中剩余的），就把这10个区块返回，第1个交给客户，余9个由free_list[7]维护 
- 3.此时内存池全空。接下来再调用chunk_alloc(96,20)，此时free-list[11]空空如也，必须向内存池申请。而内存池此时也为空，于是以malloc()分配40+n(附加量)个96bytes区块，其中第1个交出，另19个交给free-list[11]维护，余20+n(附加量)个区块留给内存池... 

![](../../pics/language/STL源码剖析/img-2-7-内存池例子.png)

## 2.3 内存基本处理工具

STL定义了5个全局函数，作用于未初始化空间上：

- 1.construct()
- 2.destroy()
- 3.uninitialized_copy()，对应于高层函数copy()
- 4.uninitialized_fill()，对应于高层函数fill()
- 5.uninitialized_fill_n()，对应于高层函数fill_n()

本节分析后3个函数，如果要使用后3个函数，应该包含[<memory>](../../source/STL/g++/memory)，但SGI把它们实际定义于[<stl_uninitialized.h>](../../source/STL/g++/stl_uninitialized.h)

一些需要提前知道的知识：**POD**

POD意指Plain Old Data，也就是标量类型(scalar types)或传统的C struct类型。POD类型必然拥有trivial ctor/dtor/copy/assignment函数。因此，可以对POD类型采用最有效的初值填写手法，而对non-POD类型采取最保险的做法 

**三个内存基本函数的泛型版本与特化版本的执行流程**：

![](../../pics/language/STL源码剖析/img-2-8-三个内存基本函数的泛型版本与特化版本.png)

### 2.3.1 uninitialized_copy

```c++
template <class InputIterator, class ForwardIterator>
inline ForwardIterator
  uninitialized_copy(InputIterator first, InputIterator last, ForwardIterator result);
```

**三个参数**：

- 1.迭代器first：指向输入端的起始位置
- 2.迭代器last：指向输入端的结束位置（前闭后开区间）
- 3.迭代器result：指向输出端（欲初始化空间）的起始处

uninitialized_copy将内存的配置与对象的构造行为分离开（本函数完成“构造行为”），如果作为输出目的地的[result, result+(last-first))范围内的每一个迭代器i都未初始化，该函数会调用`construct(&*(result+(i-first)),*i)`，产生*i的复制品，放置于输出范围的相对位置上

uninitialized_copy的**用处**：容器的全区间构造函数通常调用该函数来完成，具体过程如下：

- 1.配置内存区块，足以容纳范围内的所有元素
- 2.使用uninitialized_copy()，在该内存区块上构造元素

uninitialized_copy具有**commit or rollbak语意**：要么构造所有元素，要么不构造任何元素，如果任何一个拷贝构造函数丢出异常，必须能够将所有元素析构掉

**uninitialized_copy源码分析**：

```
uninitialized_copy
	---> __uninitialized_copy //判断first是否为POD
		---> __uninitialized_copy_aux(...,__true_type) //first为POD
			---> copy(first, last, result);//交给高阶函数执行，STL的copy()
		---> __uninitialized_copy_aux(...,false_type) //first不是POD
			---> construct(&*cur, *first);
uninitialized_copy(const char* first,...) //针对first为char*的特化版本
uninitialized_copy(const wchar_t* first,...) //针对first为wchar_t*的特化版本
```

[uninitialized_copy()的实现](STL/uninitialized_copy().md)

### 2.3.2 uninitialized_fill

```c++
template <class ForwardIterator, class T>
inline void uninitialized_fill(ForwardIterator first, ForwardIterator last, const T& x);
```

**三个参数**：

- 1.迭代器first：指向输出端（欲初始化空间）的起始处
- 2.迭代器last：指向输出端（欲初始化空间）的结束处（前闭后开区间）
- 3.x：表示初值

uninitialized_fill将内存的配置与对象的构造行为分离开（本函数完成“构造行为”），如果作为输出目的地的[first, last)范围内的每一个迭代器i都未初始化，该函数会调用`construct(&*i,x)`，在i所指之处产生x的复制品

uninitialized_fill具有**commit or rollbak语意**：要么构造所有元素，要么不构造任何元素，如果任何一个拷贝构造函数丢出异常，必须能够将所有元素析构掉

**uninitialized_fill源码分析**：

```
uninitialized_fill
	---> __uninitialized_fill //判断first是否为POD
		---> __uninitialized_fill_aux(...,__true_type) //first为POD
			---> fill(first, last, x);//交给高阶函数执行，调用STL算法fill()
		---> __uninitialized_fill_aux(...,false_type) //first不是POD
			---> construct(&*cur, x);
```

[uninitialized_fill()的实现](STL/uninitialized_fill().md)

### 2.3.3 uninitialized_fill_n

```c
template <class ForwardIterator, class Size, class T>
inline ForwardIterator uninitialized_fill_n(ForwardIterator first, Size n,const T& x);
```

**三个参数**：

- 1.迭代器first：指向初始化空间的起始处
- 2.n：表示欲初始化空间的大小
- 3.x：表示初值

uninitialized_fill_n将内存的配置与对象的构造行为分离开（本函数完成“构造行为”）。该函数为指定范围内的所有元素设定相同的初值。如果[first, first+n)范围内的每一个迭代器i都未初始化，该函数会调用`construct(&*i,x)`，在i所指之处产生x的复制品

uninitialized_fill_n具有**commit or rollbak语意**：要么构造所有元素，要么不构造任何元素，如果任何一个拷贝构造函数丢出异常，必须能够将所有元素析构掉

**uninitialized_fill_n源码分析**：

```
uninitialized_fill_n
	---> __uninitialized_fill_n //判断first是否为POD
		---> __uninitialized_fill_n_aux(...,__true_type) //first为POD
			---> fill_n(first, n, x);//交给高阶函数执行
		---> __uninitialized_fill_n_aux(...,false_type) //first不是POD
			---> construct(&*cur, x);
```

[uninitialized_fill_n()的实现](STL/uninitialized_fill_n().md)

# 第3章 迭代器概念与traits编程技法

iterator模式定义：提供一种方法，使之能够依序巡防某个容器所含的各个元素，而又无需暴露该容器的内部表述方式

迭代器是一种“智能指针”

STL容器都提供专属迭代器

迭代器最重要的编程工作是对operator*（解引用）和operator->（成员访问）进行重载

## 3.3 迭代器相应类别

**相应类别**：迭代器所指之物的类别是相应类别的一种

**需求**：假设算法中有必要声明一个变量，以“迭代器所对象的类别”为类型，如何是好？C++不支持`typeof()`，而`typeid()`只能获得类型名称，而不能用来声明变量

**解决方法**：

- 1.**函数模板的参数推导机制**
  - 缺点：无法用于函数的返回类型声明
- 2.**声明内嵌类型**
  - 缺点：无法用于非类类型，如原生指针。因为非类类型无法声明内嵌类型
- 3.**偏特化**
  - 优点：解决非类类型的“相应类别”萃取

下面对这三个方法详细说明

> **1.函数模板的参数推导机制**

![](../../pics/language/STL源码剖析/img-3-函数模板的参数推导机制.png)

编译器会自动进行template参数推导，于是导出类别T，解决了问题

**缺点**：迭代器所指对象的类别，称为该迭代器的value type。该方法无法将value type用于返回值的类型声明。函数的“模板推导机制”只能推导参数，无法推导函数的返回值类别

> **2.声明内嵌类型**

![](../../pics/language/STL源码剖析/img-3-声明内嵌类型.png)

**注意**：func()的返回类别必须加上关键字typename，因为T是一个模板参数，在它编译器具现化之前，编译器对T一无所知，编译器此时并不知道`MyIter<T>::value_type`代表的是一个类别或是一个成员函数或数据成员。关键字typename的用意在于告诉编译器这是一个类别，如此才能顺利通过编译

**缺点**：并不是所有迭代器都是类类型，原始指针就不是。如果不是类类型，就无法为它定义内嵌类别。但STL绝对必须接受原生指针作为一种迭代器

> **3.偏特化**

解决**非类类型的迭代器**的**相应类别**的萃取

- 指针对迭代器的模板参数为指针，设计特化版本的迭代器
- 针对**指向常数对象的指针**，设计特化版本的迭代器

## 3.4 Traits编程技法

使用3.3节提到的后两种方法实现`iterator_traits`类，该类专门用来“萃取”迭代器的特性

> 常用的迭代器相应类别

最常用到的迭代器相应类别有五种：value type、difference type、pointer、reference、iterator、iterator catagoly，如果你希望你所开发的容器与STL水乳交融，一定要为你的容器的迭代器定义这五种对应类别。“特性萃取机”traits会很忠实地将原汁原味榨取出来

> Traits的实现

```c++
//声明内嵌类型
template <class Iterator>
struct iterator_traits {
  typedef typename Iterator::iterator_category iterator_category;
  typedef typename Iterator::value_type        value_type;
  typedef typename Iterator::difference_type   difference_type;
  typedef typename Iterator::pointer           pointer;
  typedef typename Iterator::reference         reference;
};

//偏特化：原生指针
template <class T>
struct iterator_traits<T*> {
  typedef random_access_iterator_tag iterator_category;
  typedef T                          value_type;
  typedef ptrdiff_t                  difference_type;
  typedef T*                         pointer;
  typedef T&                         reference;
};

//偏特化：指向常数对象的指针
template <class T>
struct iterator_traits<const T*> {
  typedef random_access_iterator_tag iterator_category;
  typedef T                          value_type;
  typedef ptrdiff_t                  difference_type;
  typedef const T*                   pointer;
  typedef const T&                   reference;
};
```

> Traits的使用

```c++
template<class I>
typename iterator_traits<I>::value_type  //函数的返回类型
func(I ite)
{
    return *ite;
}
```

 ![](../../pics/language/STL源码剖析/img-3-1.png)





**五种迭代器相应型别**：

> **1.value type**

**作用**：value type是指迭代器所指对象的类别

> **2.difference type**

**作用**：difference type用来表示两个迭代器之间的距离，也可用来表示一个容器的最大容量

**例子**：一个泛型算法提供计数功能，例如STL的count()，其传回值就必须使用迭代器的diffrence type

```c++
template<class I, class T>
typename iterator_traits<I>::difference_type count(){}
```

**特例化**：原生指针和“指向常数对象的指针”的difference_type都为C++内建的**ptrdiff_t**（定义于<cstddef>头文件）

> **3.reference type**

从“迭代器所指之物的内容是否允许改变”的角度看，迭代器分为两种：

- 1.**constant iterators**：不允许改变“所指对象之内容”的迭代器
  - 如：`const int* pic;`
- 2.**mutable iterators**：允许改变“所指对象之内容”的迭代器
  - 如`int* pi;`

函数如果要传回左值，都是以传引用的方式进行

- 当p是一个mutable iterators时，如果value type是T，那么*p的reference type为T&
- 当p是一个constant iterators时，如果value type是T，那么*p的reference type为const T&

> **4.pointer type**

- 当p是一个mutable iterators时，如果value type是T，那么`*p`的pointer type为`T*`
- 当p是一个constant iterators时，如果value type是T，那么`*p`的pointer type为`const T*`

> **5.iterator_category**

根据移动特性与实施操作，迭代器被分为五类

- 1.Input Iterator：只读迭代器，只能向前移动（operator++）
- 2.Output Iterator：只写迭代器，只能向前移动（operator++）
- 3.Forward Iterator：读写迭代器，只能向前移动（operator++）
- 4.Bidirectional Iterator：双向移动迭代器（operator++、operator--）
- 5.Random Access Iterator：随机访问迭代器（operator++、oprerator--、p+n、p-n、p[n]、p1-p2、p1<p2）

这些迭代器的**分类与从属关系**如下图：

![](../../pics/language/STL源码剖析/img-3-2.png)

设计算法时，如果可能，尽量针对上图中的某种迭代器提供一个明确定义，并针对更强化的某些迭代器提供另一种定义，这样才能在不同情况下提供最大效率

[以advance()为例](STL/以advance()为例.md)

## 3.5 iterator 的保证

为了符合规范，任何迭代器都应该提供5个内嵌相应类型，以便于traits萃取，否则便是自别于整个STL架构，可能无法与其它STL组件顺利搭配。然而，写代码难免会有遗漏。因此，STL提供了一个iterators class如下，如果每个新设计的迭代器都继承自它，就可保证符合STL所需的规范

```c++
template <class Category,
          class T,
          class Distance = ptrdiff_t,
          class Pointer = T*,
          class Reference = T&>
struct iterator{
    typedef Category    iterator_category;
    typedef T           value_type;
    typedef Distance    difference_type;
    typedef Pointer     pointer;
    typedef Reference   reference;
};
```

iterator class不含任何成员，存粹只是类型定义，所以继承它不会导致任何额外负担。由于后3个参数皆有默认值，故新的迭代器只需提供前2个参数即可。以下为一个继承示例： 

```c++
template <class Item>
struct ListIter : public std::iterator<std::forword_iterator_tag, Item>{
    ...
};
```

**总结**：

- 1.设计适当的相应类型，是迭代器的责任
- 2.设计适当的迭代器，是容器的责任
- 3.算法，完全可以独立于容器和迭代器之外自行发展，只要设计时以迭代器为对外接口就行

## 3.7 SGI STL 的私房菜：__type_traits

STL只对迭代器加以规范，指定出**iterator_traits**。SGI把这种技法进一步扩大到迭代器以外的世界，于是有了**__type_traits**

- **iterator_traits**：负责萃取**迭代器**的特性
- **__type_traits**：负责萃取**类型**的特性

**__type_traits萃取的类型特性有**：

- 1.是否具备“有用的默认构造函数”（non-trivial default constructor）
- 2.是否具备“有用的拷贝构造函数”（non-trivial copy constructor）
- 3.是否具备“有用的赋值操作运算符”（non-trivial assignment operator）
- 4.是否具备“有用的析构函数”（non-trivial destructor）
- 5.是否为POD

如果被萃取的类型，前4种特性都是“无用的”，在对这个类别进行构造、拷贝、赋值、析构时，就可以采用最有效的措施（如根本不调用那些“无用的”相关函数），而采用内存直接处理操作如malloc()、memcopy()等等，以获得最高效率

> __type_traits的运用

```c++
__type_traits<T>::has_trivial_default_constructor
__type_traits<T>::has_trivial_copy_constructor
__type_traits<T>::has_trivial_assignment_operator
__type_traits<T>::has_trivial_destructor trivial_destructor
__type_traits<T>::is_POD_type
```

> __type_traits的返回类型

上述式子应该传回这样的东西

```c++
struct __true_type { };
struct __false_type { };
```

> __type_traits的定义

```c++
template <class type>
struct __type_traits { 
   typedef __true_type     this_dummy_member_must_be_first;

   typedef __false_type    has_trivial_default_constructor;
   typedef __false_type    has_trivial_copy_constructor;
   typedef __false_type    has_trivial_assignment_operator;
   typedef __false_type    has_trivial_destructor;
   typedef __false_type    is_POD_type;
};
```

把所有内嵌类型都默认定义为`__false_type`的原因：SGI定义最保守的值（即被萃取的类型具有“有用的”特性），然后再针对每个标量类型设计适当的`__type_traits`特例化版本。`<type_traits.h>`内对所有C++标量类型提供了对应的特化声明

上述`__type_traits`可以接收任何类型的参数，五个typedefs将经由以下管道获得实值：

- 1.**一般实例**：上述各个`has_trivial_xxx`都被定义为`__false_type`
- 2.经过声明的**特化版本**
- 3.某些编译器会自动为所有类型提供适当的特化版本（但作者怀疑其精确程度）

> <type_traits.h>

`<type_traits.h>`对以下类型提供了特化版本

- char、signed char、unsigned char
- short、unsigned short
- int、unsigned int
- long unsigned long
- float、double、long double

[<type_traits.h>中的特例化](STL/type_traits.h中的特例化.md)

`<type_traits.h>`有如下声明

```c++
/* 注意：本文件是内部头文件，被其他STL头文件包含
*  你不应该尝试直接使用该文件
*/
```

> __type_traits的例子

[__type_traits例子---uninitialized_fill_n()](STL/__type_traits例子.md)

> 对于SGI STL用户

如果你是SGI STL用户，可以在自己的程序中充分运用__type_traits

假设定义了一个Shap类，如果想要使用`__type_traits`，则必须定义`__type_traits`关于Shape的特化版本

```c++
template<> struct __type_traits<Shape> {...}
```

# 第4章 序列式容器

![](../../pics/language/STL源码剖析/img-4-1.png)

上图中的“衍生”并非“派生”，而是内含关系。例如heap内含一个vector，priority-queue内含一个heap，stack和queue都含一个deque，set/map/multiset/multimap都内含一个RB-tree，has_x都内含一个hashtable 

## 4.2 vector

### 4.2.1 vctor 概述

array与vector的**区别**：空间的运用的灵活性

- 1.array是静态空间，一旦配置就不能更改
- 2.vector是动态空间，随着元素的加入，它的内部机制会自行扩充空间以容纳新元素

### 4.2.2 vector 定义摘要 

STL规定，欲使用vector者必须先包含`<vector>`，但SGI STL将vector实现于更底层的`<stl_vector.h>`

[vector的定义]()

### 4.2.3 vector的迭代器

vector的迭代器就是普通指针，因为：

- 1.vector维护的是一个连续线性空间
- 2.vector所需要的操作如`*、->、++、--、+、-、+=、-=`，普通指针都具备
- 3.vector支持随机存取，普通指针拥有这样的能力

因此，vector提供的是Random Access Iterators

> 定义

```c++
template <class T, class Alloc = alloc>
class vector {
public:
  typedef T value_type;
  typedef value_type* iterator;
};
```

> 例子

```c++
//ivite的类型为int*
vector<int>::iterator ivite;
//svite的类型为Shape*
vector<int>::iterator svite;
```

### 4.2.4 vector的数据结构

> 数据结构

**数据结构**：vector采用连续线性空间，以两个迭代器start和finish分别指向配置得来的连续空开中目前已经被使用的范围，以迭代器end_of_storage指向整块连续空间（含备用空间）的尾端

**容量**：为了降低空间配置时的速度成本，vector实际配置的大小可能比客户端需求量更大一些，以备将来可能的扩充。一旦**容量**等于**大小**，便是**满载**，下次再有新增元素，整个vector就得另觅居所

```c++
class vector {
  //表示目前使用空间的头
  iterator start;
  //表示目前使用空间的尾
  iterator finish;
  //表示目前可用空间的尾
  iterator end_of_storage;
};
```

> 大小、容量相关函数

运用start、finish、end_of_storage三个迭代器，提供首尾迭代器、大小、容量、空容器判断、[]运算、最前端元素值、最后端元素值等功能

```c++
class vector {
public:
  iterator begin() { return start; }
  iterator end() { return finish; } 
  size_type size() const { return size_type(end() - begin()); }
  size_type capacity() const { return size_type(end_of_storage - begin()); }
  bool empty() const { return begin() == end(); }
  reference operator[](size_type n) { return *(begin() + n); }
  reference front() { return *begin(); }
  reference back() { return *(end() - 1); }
}；
```

![](../../pics/language/STL源码剖析/img-4-2.png)

### 4.2.5 vector的构造

> constructor

vector提供许多构造函数，其中一个允许我们制定空间大小及初值：

```c++
//允许指定vector大小n、初值value、容量n
vector(size_type n, const T& value) { fill_initialize(n, value); }

//填充并予以初始化
void fill_initialize(size_type n, const T& value) {
  start = allocate_and_fill(n, value);
  //大小为n
  finish = start + n;
  //容量为n
  end_of_storage = finish;
}

iterator allocate_and_fill(size_type n, const T& x) {
  //配置n个元素空间
  iterator result = data_allocator::allocate(n);
  //以x为初值，从result位置开始构造n个元素，参考2.2.3节
  uninitialized_fill_n(result, n, x);
  return result;
}
```

### 4.2.6 vector的元素操作

- 1.push_back
- 2.pop_back
- 3.erase
- 4.clear
- 5.insert

> **1.push_back**

push_back()将新元素插入于vector尾端时，该函数首先检查是否有备用空间

- 如果有，直接在备用空间上构造元素，并调整迭代器finish，使vector变大
- 如果没有，扩充空间：重新配置、移动数据、释放原空间

**分配过程**：

- 1.**计算新空间**，配置原则：
  - 如果原大小为0，则配置1个元素大小
  - 如果原大小不为0，则配置原大小的两倍
  - 前半段用来放置原数据，后半段准备用来放置新数据
- 2.进行空间配置
- 3.将原vector的内容拷贝到新vector
- 4.构造新元素，并放入vector中
- 5.释放原vector
- 6.更新start、finish、end_of_storage迭代器

[push_back()的实现](STL/vector-push_back().md)

**注意**：

- 1.动态增加并不是在原空间之后接续新空间（因为无法保证原空间之后尚有可配置的空间），而是以原大小的两倍**另外**配置一块较大空间
- 2.对vector的任何操作，一旦引起**空间重新配置**，指向原vector的所有迭代器就都失效了

> **2.pop_back()**

```c++
//将尾元素拿掉，并调整大小
void pop_back() {
  //将尾端标记往前移一格，表示将放弃尾端元素
  --finish;
  destroy(finish);
}
```

> **3.erase()**

两个重载版本

**版本1 erase(first,last)**：

![](../../pics/language/STL源码剖析/img-4-3-a.png)

```c++
//清除[first,last)中的元素
iterator erase(iterator first, iterator last) {
  //copy三个参数分别是：源起始位置、源终止位置（不包含）、目的起始位置
  //这部的操作是将last之后的元素，覆盖到fisrt来
  //返回值i为上图下部分，vector<T>::finish所指位置
  iterator i = copy(last, finish, first);
  //将i之后的元素析构
  destroy(i, finish);
  finish = finish - (last - first);
  return first;
}
```

**版本2 erase(position)**：

```c++
//清除某个位置上的元素
iterator erase(iterator position) {
  //如果清除的不是尾元素之前的元素，需要将后面的数据覆盖上来
  if (position + 1 != end())
    copy(position + 1, finish, position);
  --finish;
  destroy(finish);
  return position;
}
```

> **4.clear()**

```c++
void clear() { erase(begin(), end()); }  
```

>  **5.insert()**

**实现过程**：

前提：当n不等于0时，才进行以下所有操作（n为新增元素个数）

- (1)：如果备用空间大于等于“新增元素个数”，计算插入点之后的现有元素个数
  - (1-1)：如果“插入点之后的现有元素个数”大于“新增元素个数”
    - 1.先将“插入点之后的现有元素”的后n个移动到finish之后
    - 2.更新finish
    - 3.再将“插入点之后的现有元素”剩下的前几个元素向后移动
    - 4.将新加入的元素从“插入点”填入
  - (1-2)：“插入点后的现有元素个数”小于等于“新增元素个数”
    - 1.在“现有元素”之后先添加“新增元素个数”比“插入点后的现有元素个数”多的数 
    - 2.更新finish
    - 3.将“插入点后的现有元素”全部拷贝到新finish之后
    - 4.更新finish
    - 5.填入剩余“新增元素个数”
- (2)：备用空间小于“新增元素个数”（那就必须配置额外的内存），首先决定新长度：旧长度的两倍，或旧长度+新增元素个数
  - 1.配置新的vector空间
  - 2.首先将旧vector的插入点之前的元素复制到新空间
  - 3.再将新增元素（初值皆为n）填入新空间
  - 4.最后将旧vector的插入点之后的元素赋值到新空间
    - 如果有异常发生，实现“commit or rollback”语意
    - 若新空间分配成功，清除并释放旧vector空间
      - 更新start、finish

[insert()的实现](STL/vector-insert().md)

**insert(position,n,x)**

- (1)备用空间 2 >= 新增元素个数2
  - (1-1)插入点之后的现有元素个数3 > 新增元素个数2

![](../../pics/language/STL源码剖析/img-4-3b-1.png)

- (1)备用空间 3 >= 新增元素个数3
  - (1-2)插入点之后的现有元素个数2 <= 新增元素个数3

![](../../pics/language/STL源码剖析/img-4-3b-2.png)

- (2)备用空间2 < 新增元素个数3

![](../../pics/language/STL源码剖析/img-4-3b-3.png)

## 4.3 list

### 4.3.1 list概述

**优势**：

- 1.每次插入或删除一个元素，就配置或释放一个元素空间，对空间的运用绝对的精准，一点也不浪费
- 2.对于任何位置的元素插入或元素移除，list永远是常数时间

### 4.3.2 list的节点

list本身和list的节点是不同的结构，需要分开设计

STL list的节点（node）结构，一个双向链表：

```c++
template <class T>
struct __list_node{
    typedef void* void_pointer;
    //类型为void*，其实可设为 __list_node<T>*
    void_pointer prev;  
    void_pointer next;
};
```

![](../../pics/language/STL源码剖析/img-4-list_node.png)

### 4.3.3 list的迭代器

STL list是一个双向链表，迭代器必须具备前移、后移的能力，所以list提供的是一个**Bidirectional Iterators**

**迭代器失效问题**

- 1.插入操作（insert）、接合操作（splice）不会造成原有的list迭代器失效
- 2.删除操作（erase）只有“指向被删除元素”的那个迭代器失效，其他迭代器不受影响

![](../../pics/language/STL源码剖析/img-4-4.png)

```c++
template<class T, class Ref, class Ptr>
struct __list_iterator {
  typedef __list_iterator<T, T&, T*>             iterator;
  typedef __list_iterator<T, const T&, const T*> const_iterator;
  typedef __list_iterator<T, Ref, Ptr>           self;

  //迭代器属于Bidirectional Iterators
  typedef bidirectional_iterator_tag iterator_category;
  typedef T value_type;
  typedef Ptr pointer;
  typedef Ref reference;
  //节点指针类型link_type
  typedef __list_node<T>* link_type;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;

  //迭代器内部的指针，指向list的节点
  link_type node;

  //构造函数
  __list_iterator(link_type x) : node(x) {}
  __list_iterator() {}
  __list_iterator(const iterator& x) : node(x.node) {}

  bool operator==(const self& x) const { return node == x.node; }
  bool operator!=(const self& x) const { return node != x.node; }
  //对迭代器取值，取的是节点的数据值
  reference operator*() const { return (*node).data; }

  //以下是迭代器的成员存取运算子的标准做法
  pointer operator->() const { return &(operator*()); }

  //前置++，对迭代器累加1，就是前进一个节点
  self& operator++() { 
    node = (link_type)((*node).next);
    return *this;
  }
  //后置++
  self operator++(int) { 
    self tmp = *this;
    ++*this;
    return tmp;
  }

  //前置--，对迭代器递减1，就是后退一个节点
  self& operator--() { 
    node = (link_type)((*node).prev);
    return *this;
  }
  //后置--
  self operator--(int) { 
    self tmp = *this;
    --*this;
    return tmp;
  }
};
```

### 4.3.4 list的数据结构

SGI list不仅是一个双向链表，还是一个**环状双向链表**。所以它只需要一个指针，便可完整表现整个链表： 

```c++
template <class T, class Alloc = alloc> //确实使用alloc为配置器
class list {
protected:
    typedef __list_node<T> list_node;
public:
    typedef list_node* link_type;

protected:
    //只要一个指针，便可表示整个环状双向链表
    //指向尾端的空白节点
    link_type node; 
};

iterator begin() { return (link_type)((*node).next); }
//end()就是那个空白节点
iterator end() { return node; }
size_type size() const {
    size_type result = 0;
    distance(begin(), end(), result);
    return result;
}
```

让指针指向刻意置于尾端的一个空白节点，node便能符合STL对于“前闭后开”`[)`区间的要求，称为last迭代器

![](../../pics/language/STL源码剖析/img-4-5.png)

### 4.3.5 list的构造

> 空间配置

list缺省使用alloc作为空间分配器，并据此另外定义了一个list_node_allocator，为的是更方便以节点大小为配置单位： 

```c++
template <class T, class Alloc = alloc>
class list {
protected:
    typedef simple_alloc<list_node, Alloc> list_node_allocator;
...
};
```

> 构造函数

list提供许多构造函数，其中一个是默认构造函数，允许我们不指定任何参数做出一个空的list

```c++
public:
list() { empty_initialize(); }

protected:
  void empty_initialize() { 
    node = get_node();
    node->next = node;
    node->prev = node;
  }
```

### 4.3.6 list的元素操作

- 节点操作
  - 分配一个节点：[get_node()](STL/list-get_node().md)
  - 释放一个节点：[put_node()](STL/list-put_node().md)
  - 产生（配置并构造）一个节点：[create_node()](STL/list-create_node.md)
  - 销毁（析构并释放）一个节点：[destroy_node()](STL/list-destroy_node.md)
  - 节点插入：
    - [insert(position,x)](STL/list-insert().md)：在迭代器position之前插入一个节点x
    - [puch_back()](STL/list-push_back().md)：插入一个节点，作为尾节点
    - [push_front()](STL/list-push_front().md)：插入一个节点，作为头节点
  - 节点移除：
    - [erase(position)](STL/list-erase().md)：移除迭代器position所指节点，返回原position下一个节点的迭代器
    - [pop_front()](STL/list-pop_front().md)：移除头节点
    - [pop_back()](STL/list-pop_back().md)：移除尾节点
    - [remove(value)](STL/list-remove().md)：将数值为value的所有元素移除
    - [unique()](STL/list-unique().md)：移除数值相同的连续节点，注意，只有“连续而相同的元素”，才会被移除剩一个
- 链表操作
  - [list()](STL/list-list().md)：list构造函数，创建一个空链表
  - [clear()](STL/list-clear().md)：清除所有节点（整个链表）
  - [transfer(position,first,last)](STL/list-transfer().md)：内部接口，迁移操作，将[first,last)内的所有元素移动到position之前
    - [first,last)区间可以是同一list，也可以是不同list
    - 该函数并非公开接口，list提供的接合操作是splice()
  - [splice()](STL/list-splice().md)：链表拼接，将一个list中的连续范围的元素移动到另一个（或同一个）list的某个定点，有三个重载形式。内部调用transfer()
  - [merge()](STL/list-merge().md)：将一个链表合并到另一个链表，两个链表的内容需先经过递增排序，合并后链表也是递增排序的
  - [reverse()](STL/list-reverse().md)：将链表逆向重置
  - [sort()](STL/list-sort().md)：list不能使用STL算法sort()，必须使用自己的sort()成员函数，因为STL算法sort()只接受RamdonAccessIterator。本函数采用快排

## 4.4 deque

### 4.4.1 deque概述

deque是一种双向开口的连续线性空间，可以在头尾两端分别做元素的插入和删除操作

**deque与vector的差异**：

- 1.deque允许于常数时间内对起头端进行元素的插入或移除操作 
- 2.deque没有所谓容量观念，因为它是动态地以分段连续空间组合而成，随时可以增加一段新的空间并链接起来（deque没有必要提供所谓的空间保留功能） 
- 3.vector的迭代器是普通指针，但deque的迭代器不是，因此deque迭代器效率较低

**对deque进行排序的建议**：为了最高效率，可将deque先完整复制到一个vector，将vector排序后（利用STL sort算法），再复制回deque

### 4.4.2 deque的中控器

deque系由一段一段的定量连续空间构成，一旦有必要在deque的前端或尾端增加新空间，便配置一段定量连续空间，串接在整个deque的头端或尾端。

- **好处**：避开了“重新配置、复制、释放”的轮回（但map满了，仍然有这个过程）
- **坏处**：复杂的迭代器架构

deque采用一块所谓的map（注意，不是STL的map容器）作为主控，这里所谓的map是一小块连续空间，其中每个元素（此处称为一个节点，node）都是指针，指向另一端（较大的）连续线性空间，称为**缓冲区**。缓冲区才是deque的存储空间本身。SGI STL允许指定缓冲区大小，默认为0表示将使用512字节缓冲区

```c++
//BufSiz默认为0，表示512字节
template <class T, class Alloc = alloc, size_t BufSiz = 0> 
class deque {
public:                         // Basic types
  typedef T value_type;
  typedef value_type* pointer;
protected:                      // Internal typedefs
  //元素的指针的指针，指向的map的指针类型
  typedef pointer* map_pointer;
  
  //指向map，map是块连续空间，
  //其内的每个元素都是一个指针（称为节点），指向一块缓冲区
  //map是一个T**，即指针的指针
  map_pointer map;
  //map可以容纳多少指针  
  size_type map_size;
```

![](../../pics/language/STL源码剖析/img-4-10.png)

### 4.4.3 deque的迭代器

deque是分段连续空间，维持其“整体连续”假象的任务，落在了迭代器的operator++和operator--两个运算身上

迭代器必须完成的工作：

- 1.必须能够指出分段连续空间（即缓冲区）在哪 
- 2.必须能够判断自己是否已经处于其所在缓冲器的边缘。如果是，一旦前进或后退时就必须跳跃至下一个或上一个缓冲区
- 3.为了能够正确跳跃，迭代器必须随时掌握中控器map 

```c++
template <class T, class Ref, class Ptr, size_t BufSiz>
struct __deque_iterator {   //未继承std::iterator
  typedef __deque_iterator<T, T&, T*, BufSiz>             iterator;
  typedef __deque_iterator<T, const T&, const T*, BufSiz> const_iterator;
  //buffer_size()用来决定缓冲区大小
  static size_t buffer_size() {
  //__deque_buf_size是一个全局函数，对该函数的解释如下：
  //如果BufSiz不为0，传回BufSiz（个缓冲区），表示buffer size由用户自定义
  //如果BufSiz为0，表示buffer size使用默认值，那么
  //  如果sizeof(T)小于512，则传回 512/sizeof(T) （个缓冲区）
  //  如果sizeof(T)不小于512，则传回1 （个缓冲区）
  return __deque_buf_size(BufSiz, sizeof(T)); 
  }

  //未继承std::iterator，所以必须自行撰写5个必要的迭代器相应类型
  typedef random_access_iterator_tag iterator_category; // (1)
  typedef T value_type;                                 // (2)
  typedef Ptr pointer;                                  // (3)
  typedef Ref reference;                                // (4)
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;                    // (5)
  typedef T** map_pointer;

  typedef __deque_iterator self;

  //保持与容器的联结
  T* cur;           //此迭代器所指缓冲区中的当前元素
  T* first;         //此迭代器所指缓冲区的头
  T* last;          //此迭代器所指缓冲区的尾(含备用空间)
  map_pointer node; //指向中控器map
...
};
```

![](../../pics/language/STL源码剖析/img-4-11.png)

- [set_node()](STL/deque-set_node().md)：更新迭代器指向的缓冲区
- [operator*()](STL/deque-解引用函数.md)
- [operator->()](STL/deque-成员访问函数.md)
- [operator-(x)](STL/deque-operator-(x).md)：计算两个迭代器之间的距离
- [operator++()](STL/deque-operator++().md)和[operator++(int)](STL/deque-operator++(int).md)
- [operator--()](STL/deque-operator--().md)和[operator--(int)](STL/deque-operator--(int).md)
- [operator+(n)](STL/deque-operator+(n).md)
- [operator+=(n)](STL/deque-operator+=(n).md)：实现随机存取，迭代器可直接跳跃n个距离
- [operator-(n)](STL/deque-operator-(n).md)
- [operator-=(n)](STL/deque-operator-=(n).md)
- [`operator[]()`](STL/deque-operator[]().md)
- [operator==()](STL/deque-operator==().md)
- [operator!=()](STL/deque-operator!=().md)
- [operator<()](STL/deque-小于函数.md)

### 4.4.4 deque的数据结构

- 1.start、finish两个迭代器，分别指向第一缓冲区的第一个元素和最后缓冲区的最后一个元素（的下一个位置）
- 2.map_size记住目前的map大小，一旦map提供的节点不足，就重新配置一块更大的map

```c++
template <class T, class Alloc = alloc, size_t BufSiz = 0> 
class deque {
public:                         // Basic types
  typedef T value_type;
  typedef value_type* pointer;
  typedef const value_type* const_pointer;
  typedef value_type& reference;
  typedef const value_type& const_reference;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;
    
  typedef __deque_iterator<T, T&, T*, BufSiz>              iterator;
  typedef __deque_iterator<T, const T&, const T&, BufSiz>  const_iterator;

protected:                      // Internal typedefs
  //元素的指针的指针，指向的map的指针类型
  typedef pointer* map_pointer;

protected:                      // Data members
  iterator start;   //表现第一个节点
  iterator finish;  //表现最后一个节点

  map_pointer map;  //指向map，map是块连续空间
                    //其每个元素都是个指针，指向一个节点（缓冲区）
  size_type map_size; //map内有多少个指针

```

> [deque-数据结构相关函数](STL/deque数据结构相关函数.md)

- begin()：返回start迭代器
- end()：返回finish迭代器
- operator[]：调用迭代器的operator[]
- front()：解引用start迭代器，获取start所指向的元素
- back()：解引用finish迭代器的上一个迭代器，获取finish上一个元素
- size()：计算元素个数
- max_size()：可容纳最大元素个数，返回的是无限大`size_t(-1)`
- empty()：判断容器是否为空

### 4.4.5 deque的构造

> 例子

```c++
deque<int,alloc,8> ideq(20,9);
```

其缓冲区大小为8（个元素，32字节），并令其保留20个元素空间，每个元素初值为9

> 空间配置器

deque自行定义了两个专属的空间配置器

```c++
protected:
    //专属的空间分配器，每次分配一个元素大小，为分配元素准备
    typedef simple_alloc<value_type,Alloc> data_allocator;
    //专属的空间分配器，每次分配一个指针大小，为分配map中的指针准备
    typedef simple_alloc<pointer,Alloc> map_allocator;
```

> [构造函数](STL/deque-构造函数.md)

提供一个构造函数

```c++
  deque(int n, const value_type& value)
    : start(), finish(), map(0), map_size(0)
  {
    fill_initialize(n, value);
  }
```

调用流程：

```
deque()
	---> fill_initialize() //负责生产并安排好deque的结构，并将初值设定妥当
		---> create_map_and_nodes() //负责产生并安排好deque的结构
```

### 4.4.6 deque的元素操作

- 1.push_back()
- 2.push_front()
- 3.pop_back()
- 4.pop_front()
- 5.clear()：清除整个deque。注意：deque的最初状态（无任何元素时）保有一个缓冲区，因此clear()完之后恢复初始状态，也一样要保留一个缓冲区
- 6.erase()：有两个重载版本，都采用覆盖方法
  - erase(pos)：清除pos迭代器指向的元素，返回原pos之后的元素的迭代器
  - erase(first,last)：清除[first,last)区间内的所有元素，返回原last指向的元素的迭代器
- 7.insert(pos,x)：允许在pos之前插入一个元素x，返回新插入元素的迭代器

[deque的元素操作](STL/deque-元素操作)

## 4.5 stack

### 4.5.1 stack概述

- 先进后出
- stack不允许有遍历行为

### 4.5.2 stack定义完整列表

- SGI STL以deque作为缺省情况下的stack底部结构 
- stack系以底部容器完成其所有工作，而具有这种“修改某物接口，形成另一种风格”的性质者，称为adapter（适配器）。其不归为container，而归为conatiner adapter
- 相关函数：
  - empty()
  - size()
  - top()
  - push()
  - pop()

stack的定义如下：

```c++
template <class T, class Sequence = deque<T> >
class stack {
  //以下__STL_NULL_TMPL_ARGS会展开为 <>
  friend bool operator== __STL_NULL_TMPL_ARGS (const stack&, const stack&);
  friend bool operator< __STL_NULL_TMPL_ARGS (const stack&, const stack&);
public:
  typedef typename Sequence::value_type value_type;
  typedef typename Sequence::size_type size_type;
  typedef typename Sequence::reference reference;
  typedef typename Sequence::const_reference const_reference;
protected:
  Sequence c;   //底层容器
public:
  //以下完全利用Sequence c的操作，完成stack的操作
  bool empty() const { return c.empty(); }
  size_type size() const { return c.size(); }
  reference top() { return c.back(); }
  const_reference top() const { return c.back(); }
  //deque是两头可进出，stack是后进后出
  void push(const value_type& x) { c.push_back(x); }
  void pop() { c.pop_back(); }
};

template <class T, class Sequence>
bool operator==(const stack<T, Sequence>& x, const stack<T, Sequence>& y) {
  return x.c == y.c;
}

template <class T, class Sequence>
bool operator<(const stack<T, Sequence>& x, const stack<T, Sequence>& y) {
  return x.c < y.c;
}
```

### 4.5.3 stack没有迭代器

stack不提供走访功能，因此不提供迭代器

### 4.5.4 以list作为stack的底层容器

只要底层容器实现了empty、size、back、push_back、pop_back函数，都可以作为stack的底层容器。如list

```c++
stack<int,list<int>> istack; 
```

## 4.6 queue

### 4.6.1 queue概述

- 先进先出
- queue不允许有遍历行为

### 4.6.2 queue定义完整列表

- SGI STL以deque作为缺省情况下的queue底部结构
- queue称为adapter（适配器）。其不归为container，而归为conatiner adapter
- 相关函数
  - empty()
  - size()
  - front()
  - back()
  - push()
  - pop()

```c++
template <class T, class Sequence = deque<T> >
class queue {
  //以下__STL_NULL_TMPL_ARGS会展开为 <>
  friend bool operator== __STL_NULL_TMPL_ARGS (const queue& x, const queue& y);
  friend bool operator< __STL_NULL_TMPL_ARGS (const queue& x, const queue& y);
public:
  typedef typename Sequence::value_type value_type;
  typedef typename Sequence::size_type size_type;
  typedef typename Sequence::reference reference;
  typedef typename Sequence::const_reference const_reference;
protected:
  Sequence c;   //底层容器
public:
  //以下完全利用Sequence c的操作，完成stack的操作
  bool empty() const { return c.empty(); }
  size_type size() const { return c.size(); }
  reference front() { return c.front(); }
  const_reference front() const { return c.front(); }
  reference back() { return c.back(); }
  const_reference back() const { return c.back(); }
  deque是两头可进出，queue是尾端紧、首部出
  void push(const value_type& x) { c.push_back(x); }
  void pop() { c.pop_front(); }
};

template <class T, class Sequence>
bool operator==(const queue<T, Sequence>& x, const queue<T, Sequence>& y) {
  return x.c == y.c;
}

template <class T, class Sequence>
bool operator<(const queue<T, Sequence>& x, const queue<T, Sequence>& y) {
  return x.c < y.c;
}
```

### 4.6.3 queue没有迭代器

queue不提供遍历功能，因此不提供迭代器

### 4.6.4 以list作为queue的底层容器

只要底层容器实现了empty、size、front、back、push_back、pop_front函数，都可以作为stack的底层容器。如list

```c++
queue<int,list<int> > iqueue;
```

## 4.7 heap

### 4.7.1 heap概述

heap并不归属与STL容器组件，它是个幕后英雄，扮演priority queue的助手 

heap是一颗完全二叉树（整颗二叉树除了底层的叶节点外，是填满的；最底层的叶节点由左至右不得有空隙），完全二叉树使用数组实现，因此使用一个vector作为heap的结构，然后通过一组xxx_heap算法，使其符合heap的性质 

**隐式表述法**：将数组的第0个元素保留（设为无限大值或无限小值），当完全二叉树中的某个节点位于数组的i处时：

- 左子节点：2i
- 右子节点：2i+1
- 父节点：i/2（取整）

**heap可分为**：

- max-heap（默认情况）：传入的比较函数为“小于函数”，因为比较时是`comp(父节点值，子节点值)`。在**上溯程序**时，将较大值往上推
- min-heap：传入的比较函数为“大于函数”

下面按照max-heap来讨论

### 4.7.2 heap算法

- 1.push_heap()
- 2.pop_heap()
- 3.sort_heap()
- 4.make_heap()

> **1.[push_heap()](STL/heap-push_heap().md)**

为满足max-heap的条件（每个节点的键值都大于等于其子节点键值），我们执行一段**上溯程序**：将新节点拿来与其父节点比较，如果其键值比父节点大，就父子对换位置。如此一一直上溯，直到不需要对换或到根节点为止

**函数执行条件**：

- 1.该函数接受两个迭代器，用来表现一个heap底部容器（vector）的头尾
- 2.新元素插入到底部容器的最尾部（调用push_back()）

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-21.png)

> **2.[pop_heap()](STL/heap-pop_heap().md)**

操作分为

- 1.pop取走根节点，放入vector的尾端节点，尾端节点原值放入临时变量
- 2.临时变量从根开始，进行下述的**下溯程序**

为满足max-heap的条件（每个节点的键值都大于等于其子节点键值），我们执行一段所谓的**下溯程序**：将空间节点和其较大子节点“对调”，并持续下放，直至叶节点为止。然后将上面说到的**被割舍元素**（原heap中，vector最后一个元素）的值设定给这个“已经到达叶层的空洞节点”，再对它指向一次**上溯程序**

**函数执行条件**：该函数接受两个迭代器，用来表现一个heap底层容器（vector）的头尾

**注意**：pop_heap之后需要push_back取走所求元素

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-22.png)

> **3.[sort_heap](STL/heap-sort_heap().md)**

**算法思路**：持续对整个heap做pop_heap操作，每次将操作范围从后向前缩减一个元素（因为pop_heap会把键值最大的元素放在底层容器的最尾端），当整个程序执行完毕时，便有了一个递增序列。**但**排序后的heap就不再是一个合法的heap

**函数执行条件**：该函数接受两个迭代器，用来表现一个heap底层容器（vector）的头尾

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-23-1.png)

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-23-2.png)

> **4.[make_heap()](STL/heap-make_heap())**

**算法作用**：将一段现有的数据转换为一个heap

**算法思路**：从最后一个有子节点的节点（最后一个非叶节点）开始（从0开始，（len-2）/2），执行**pop_heap()**中提到的**下溯程序**，直到根节点。最后还要执行一次**上溯程序**

### 4.7.3 heap没有迭代器

heap不提供遍历功能，因此不提供迭代器

## 4.8 priority_queue

### 4.8.1 priority_queue概述

priority_queue是一个拥有权值观念的queue，它允许加入新元素、移除旧元素、审视元素值等功能。只允许在底端加入元素，并从顶端取出元素

**排序次序**：priority_queue带有权值观念，其内的元素并非依照被推入的次序排序，而是自动按照元素的权值排列（通常权值以实值表示）。

**缺省情况**：缺省时priority_queue使用max-heap

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-24.png)

### 4.8.2 priority_queue定义完整列表

缺省时，以vector为底部容器，以heap为处理规则

STL priority_queue归类为container adapter

相关函数：

- priority_queue()：构造函数，利用make_heap()建堆
- empty()
- size()
- top()：利用vector的front()
- push()：先push_back()、再push_heap()
- pop()：先pop_heap()，再pop_back()

```c++
template <class T, class Sequence = vector<T>, 
          class Compare = less<typename Sequence::value_type> >
class  priority_queue {
public:
  typedef typename Sequence::value_type value_type;
  typedef typename Sequence::size_type size_type;
  typedef typename Sequence::reference reference;
  typedef typename Sequence::const_reference const_reference;
protected:
  Sequence c;       //底层容器
  Compare comp;     //元素大小比较标准
public:
  priority_queue() : c() {}
  explicit priority_queue(const Compare& x) :  c(), comp(x) {}

  //以下用到的make_heap()、push_heap()、pop_heap()都是泛型算法
  //构造一个priority queue，首先根据传入的迭代器区间初始化底层容器c，然后调用
  //make_heap()使用底层容器建堆
  template <class InputIterator>
  priority_queue(InputIterator first, InputIterator last, const Compare& x)
    : c(first, last), comp(x) { make_heap(c.begin(), c.end(), comp); }
  template <class InputIterator>
  priority_queue(InputIterator first, InputIterator last) 
    : c(first, last) { make_heap(c.begin(), c.end(), comp); }

  bool empty() const { return c.empty(); }
  size_type size() const { return c.size(); }
  const_reference top() const { return c.front(); }
  void push(const value_type& x) {
    //先利用底层容器的push_back()将新元素推入末端，再重排heap
    __STL_TRY {
      c.push_back(x); 
      push_heap(c.begin(), c.end(), comp);
    }
    __STL_UNWIND(c.clear());
  }
  void pop() {
    //从heap内取出一个元素。但不是真正弹出，而是重排heap，然后以底层容器的pop_back()
    //取得被弹出的元素
    __STL_TRY {
      pop_heap(c.begin(), c.end(), comp);
      c.pop_back();
    }
    __STL_UNWIND(c.clear());
  }
};
```

### 4.8.3 priority_queue没有迭代器

priority_queue不提供遍历功能，因此不提供迭代器

## 4.9 slist

### 4.9.1 slist概述

**STL list与slist的差别**：

- 1.list在标准规定之内，slist不在标准规定之内
- 2.list是双向链表，slist是单向链表。因此list的迭代器为Bidirectional Iterator，slist为单向的Forward Iterator

**STL list与slist相同点**：它们insert、erase、slice函数等操作不会造成原有的迭代器失效（指向被erase的那个迭代器是会失效的）

**slist的优点**：消耗空间更小，某些操作更快

**slist的缺点**：

- 1.功能受到许多限制
- 2.插入操作（insert）会将新元素插入于指定位置之前，但slist没有任何方便的办法可以回头定位出前一个位置，因此必须从头找起。除了slist起点附近的区域外，在其他位置采用insert或erase操作函数，效率很低
  - 解决方法：slist提供了insert_after()和erase_after()，即在指定迭代器之后执行相应操作

**接口特色**：（基于效率考虑）

- 1.提供了insert_after()和erase_after()，即在指定迭代器之后执行相应操作
- 2.只提供push_front()，不提供push_back()

### 4.9.2 slist的节点

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-25.png)

**节点相关的结构**：

```c++
//单向链表的节点基本结构
struct __slist_node_base
{
    __slist_node_base *next;
};

//单向链表的节点结构
template <class T>
struct __slist_node : public __slist_node_base
{
    T data;
}
```

**节点相关的全局函数**：

```c++
//已知某一节点prev_node，将新节点new_node插入其后
inline __slist_node_base* __slist_make_link(
    __slist_node_base *prev_node,
    __slist_node_base *new_node)
{
    //令new节点的下一节点为prev节点的下一节点
    new_node->next = prev_node->next;
    prev_node->next = new_node; //令prev节点的下一节点指向new节点
    return new_node;
}

//单向链表的大小（元素个数）
inline size_t __slist_size(__slist_node_base *node)
{
    size_t result = 0;
    for(;node != 0;node = node->next)
        ++result;   //一个个累计
    return result;
}
```

### 4.9.3 slist的迭代器

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-25-%E5%90%8E.png)

**迭代器的定义如下**： 

```c++
//单向链表的迭代器基本结构
struct __slist_iterator_base
{
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;
  typedef forward_iterator_tag iterator_category;   //单向

  __slist_node_base* node;  //指向节点基本结构

  __slist_iterator_base(__slist_node_base* x) : node(x) {}

  void incr() { node = node->next; }    //前进一个节点

  bool operator==(const __slist_iterator_base& x) const {
    return node == x.node;
  }
  bool operator!=(const __slist_iterator_base& x) const {
    return node != x.node;
  }
};

//单向链表的迭代器结构
template <class T, class Ref, class Ptr>
struct __slist_iterator : public __slist_iterator_base
{
  typedef __slist_iterator<T, T&, T*>             iterator;
  typedef __slist_iterator<T, const T&, const T*> const_iterator;
  typedef __slist_iterator<T, Ref, Ptr>           self;

  typedef T value_type;
  typedef Ptr pointer;
  typedef Ref reference;
  typedef __slist_node<T> list_node;

  __slist_iterator(list_node* x) : __slist_iterator_base(x) {}
  __slist_iterator() : __slist_iterator_base(0) {}
  __slist_iterator(const iterator& x) : __slist_iterator_base(x.node) {}

  reference operator*() const { return ((list_node*) node)->data; }
  pointer operator->() const { return &(operator*()); }

  self& operator++()
  {
    incr(); //前进一个节点
    return *this;
  }
  self operator++(int)
  {
    self tmp = *this;
    incr(); //前进一个节点
    return tmp;
  }
};
```

**注意**:比较两个slist迭代器是否相同时，由于`__slist_iterator`并未对`operator==`实施重写，所以会调用`__slist_iterator_base::operator==`。根据其中定义，两个slist迭代器是否相等，视其`__slist_node_base* node`是否相等

### 4.9.4 slist的数据结构

```c++
emplate <class T, class Alloc = alloc>
class slist
{
public:
  typedef T value_type;
  typedef value_type* pointer;
  typedef const value_type* const_pointer;
  typedef value_type& reference;
  typedef const value_type& const_reference;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;
  
  //定义迭代器类型
  typedef __slist_iterator<T, T&, T*>             iterator;
  typedef __slist_iterator<T, const T&, const T*> const_iterator;

private:
  typedef __slist_node<T> list_node;
  typedef __slist_node_base list_node_base;
  typedef __slist_iterator_base iterator_base;
  //定义空间配置器
  typedef simple_alloc<list_node, Alloc> list_node_allocator;
    
private:
  //头部，注意，它不是指针，是实物
  list_node_base head;
```

### 4.9.5 slist的元素操作

- 1.slist()：构造函数
- 2.~slist()：析构函数
- 3.begin()
- 4.end()
- 5.size()
- 6.empty()
- 7.swap()
- 8.front()
- 9.push_front()
- 10.pop_front()

[slist的元素操作](STL/slist的元素操作.md)

**注意**：end()的画法

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-28-%E5%90%8E.png)

# 第5章 关联式容器

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-4-1.png)

以**RB-tree**作为底层机制（自身也是关联容器，但不对外开放）

- 1.set
- 2.multiset
- 3.map
- 4.multimap

以**hash table**作为底层机制（自身也是容器，由SGI STL提供，不在标准规范之列，下面几个容器也不在）

- 1.hash_set
- 2.hash_multiset
- 3.hash_map
- 4.hash_multimap

**定义**：每个元素都有一个键值（key）和一个实值（value）。当元素被插入到关联式容器中时，容器内部结构（可能是RB-tree，也可能是hash-table）便依照其键值大小，以某种特定规则将这个元素放置于适当的位置。

**特性**：关联容器没有所谓头尾（只有最大元素和最小元素），所以不会有以下操作：

- 1.push_back()
- 2.push_frornt()
- 3.pop_back()
- 4.pop_front()
- 5.begin()
- 6.end()

**实现**：关联式容器的内部结构是一个平衡二叉树，被广泛运用于STL的是RB-tree（红黑树）

## 5.1 树的导览

**术语**：

- 1.路径长度：根节点至任何节点之间有唯一路径，路径所经过的边数
- 2.深度：根节点至任一节点的路径长度，即该节点的深度。根节点的深度为0
- 3.高度：某节点至其最深子节点（叶节点）的路径长度。树的高度即是根节点的高度
- 4.大小：任何节点的大小时指其所有子代（包括自己）的节点总数

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-2.png)

> 二叉搜索树

**插入**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-5.png)

**删除1：只有一个子节点**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-6a.png)

**删除2：有两个子节点**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-6b.png)

> 平衡二叉搜索树

“平衡”的大致意思是：没有任何一个节点深度过大

AVL-tree、RB-tree、AA-tree均可实现平衡二叉搜索树

> AVL tree

**AVL tree要求**：任何节点的左右子树高度相差最多维1

**插入后不平衡的情况**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-8.png)

**调整**：只要调整“插入点至根节点”路径上，平衡状态被破坏的各个节点中最深的那一个，便可使整棵树重新获得平衡

**“平衡被破坏”的四种分类**：（假设最深节点为X，“平衡被破坏”意味着X的左右两颗子树的高度相差2）

- 1.插入点位于X的左子节点的左子树---左左（与4对称，**外侧插入**）
- 2.插入点位于X的左子节点的右子树---左右（与3对称，**内侧插入**）
- 3.插入点位于X的右子节点的左子树---右左（与2对称，**内侧插入**）
- 4.插入点位于X的右子节点的右子树---右右（与1对称，**外侧插入**）

外侧插入解决办法：单旋转操作

内侧插入解决办法：双旋转操作

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-9.png)

**单旋转**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-10.png)

**双旋转**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-12.png)

## 5.2 RB-tree

RB-tree（红黑树）是一个二叉搜索树，并且满足下图的4个条件：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-13.png)

**另**：新增节点必须为红色

### 5.2.1 插入节点

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-14.png)

为了方便讨论，为某些特殊节点定义一些代名词

```
X:新节点
P：父节点
G：祖父节点
S：伯父节点，父节点的兄弟节点
GG：曾祖父节点

   GG
   /
   G
  / \
  P  S
 /
X
```

根据X的插入位置及外围节点（S和GG）的颜色，有四种考虑：

- 情况1：S为黑色，X为外侧插入
- 情况2：S为黑色，X为内侧插入
- 情况3：S为红色，X为外侧插入，GG为黑色
- 情况4：S为红色，X为外侧插入，GG为红色

> 情况1：S为黑色，X为外侧插入

**解决方法**：

- a：P，G做单旋转
- b：更改P，G的颜色

此时可能产生不平衡状态（高度相差1以上），这是可以的，因为RB-tree没有这么严格

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15a.png)

> 情况2：S为黑色，X为内侧插入

**解决方法**：

- a：P，X做单旋转
- b：更改G，X颜色
- c：G左单旋转

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15b.png)

> 情况3：S为红色，X为外侧插入，GG为黑色

**解决方法**：

- a：P，G做单旋转，
- b：改变X的颜色

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15c.png)

> 情况4：S为红色，X为外侧插入，GG为红色

**解决方法**：

- a：P，G做单旋转
- b：改变X的颜色
- c：持续往上做，直到不再有父子连续为红的情况

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15d.png)

### 5.2.2 一个由上而下的程序

**目的**：为了避免**情况4**”父子节点皆为红色“的情况持续向RB-tree的上层结构发展，形成处理时效上的瓶颈，可以施行一个**由上而下的程序**

**由上而下的程序**：假设新增节点A，那么沿着A的路径向上，只要看到有某个节点X的两个子节点皆为红色，就把X改为红色，并把两个子节点改为黑色

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15e.png)

**插入的操作**：

- 1.如果插入节点A的父节点P仍然为红色（此时S不可能为红），则根据情况1或情况2处理
  - 情况1：S为黑色，A为外侧插入
  - 情况2：S为黑色，A为内侧插入
- 2.如果插入节点A的父节点P不为红色
  - 要么，直接插入
  - 要么，插入，再做一次单旋转（如下图）

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15f.png)

### 5.2.3 RB-tree的节点设计

为了有更大的弹性，节点分为两层：`__rb_tree_node`继承于`__rb_tree_node_base`

minimum()和maximum()函数可以让RB-tree搜索极值变得很容易

RB-tree的各种操作时常需要上溯其父节点,因此有parent指针

```c++
typedef bool __rb_tree_color_type;
const __rb_tree_color_type __rb_tree_red = false;   //红色为0
const __rb_tree_color_type __rb_tree_black = true;  //黑色为1

//RB-tree节点的基类
struct __rb_tree_node_base
{
  typedef __rb_tree_color_type color_type;
  typedef __rb_tree_node_base* base_ptr;

  color_type color; //颜色
  base_ptr parent;  //指向父节点的指针
  base_ptr left;    //指向左子节点的指针
  base_ptr right;   //指向右子节点的指针

  //静态函数，获取以x为根节点的RB-tree最小节点的指针
  static base_ptr minimum(base_ptr x)
  {
    while (x->left != 0) x = x->left;
    return x;
  }

  //静态函数，获取以x为根节点的RB-tree最大节点的指针
  static base_ptr maximum(base_ptr x)
  {
    while (x->right != 0) x = x->right;
    return x;
  }
};

//RB-tree节点类
template <class Value>
struct __rb_tree_node : public __rb_tree_node_base
{
  typedef __rb_tree_node<Value>* link_type;
  Value value_field;    //RB-tree节点的value
};
```

**键和值都包含在value_field中** 

下面是RB-tree的节点图表，其中`__rb_tree_node::value_field`填为10

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-15f-%E5%90%8E.png)

### 5.2.4 RB-tree的迭代器

RB-tree迭代器实现为两层：`__rb_tree_iterator`继承于`__rb_tree_base_iterator`

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-16.png)

RB-tree迭代器属于双向迭代器，但不具备随机定位能力 

前进操作operator++()调用了基类迭代器的increment()，后退操作operator--()调用了基类迭代器的decrement()。前进或后退的举止行为完全依据二叉搜索树的节点排列法则 

**__rb_tree_base_iterator**：

```c++
//迭代器基类
struct __rb_tree_base_iterator
{
  typedef __rb_tree_node_base::base_ptr base_ptr;
  typedef bidirectional_iterator_tag iterator_category;
  typedef ptrdiff_t difference_type;

  base_ptr node;    //节点基类类型的指针，将迭代器连接到RB-tree的节点

  //实际上就是中序遍历的“下一个节点”问题，
  //但需要考虑RB-tree头节点的特殊情况，参考状况（4）
  void increment()
  {
    //状况（1）：如果node右子树不为空，则找到右子树的最左子节点
    if (node->right != 0) {
      node = node->right;
      while (node->left != 0)
        node = node->left;
    }
    else {
      //状况（2）：如果node右子树为空，
      //需要找到某个节点，该节点是其父节点的左孩子
      base_ptr y = node->parent;
      while (node == y->right) {
        node = y;
        y = y->parent;
      }
      //状况（3）：找到了某个节点node，该节点是其父节点y的左孩子，
      //如果node的右孩子不是其父节点y，
      //则increment的结果为找到节点node的父节点y
      if (node->right != y)
        node = y;
      //状况（4）：找到了某个节点node，该节点是其父节点y的左孩子，
      //如果node的右孩子是其父节点y，
      //则increment的结果为找到节点node，
      //当迭代器指向根节点，而根节点无右子节点时，对迭代器进行increment，
      //会进入状况（4），结果为end()，详情看下图
    }
  }
  
  //实际上就是中序遍历的“上一个节点”问题，
  //但需要考虑RB-tree头节点的特殊情况，参考状况（1）
  void decrement()
  {
    //状况（1）：这种情况发生于node为header时（亦即node为end()时）
    //header右子节点即mostright，指向max节点
    //当迭代器为end()时，若对它进行decrement，会进入状态（1），结果为max
    if (node->color == __rb_tree_red &&
        node->parent->parent == node)
      node = node->right;            //
    else if (node->left != 0) {
     //状况（2）：如果左子树不为空，则找到左子树的最右子节点
      base_ptr y = node->left;
      while (y->right != 0)
        y = y->right;
      node = y;
    }
    else {
      //状况（3）：如果左子树为空，则到某个节点node，该节点node是其父节点y的右孩子，
      //则decrement的结果为该节点的父节点y
      base_ptr y = node->parent;
      while (node == y->left) {
        node = y;
        y = y->parent;
      }
      node = y;
    }
  }
};
```

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-17.png)

**__rb_tree_iterator**：

```c++
//迭代器类
template <class Value, class Ref, class Ptr>
struct __rb_tree_iterator : public __rb_tree_base_iterator
{
  typedef Value value_type;
  typedef Ref reference;
  typedef Ptr pointer;
  typedef __rb_tree_iterator<Value, Value&, Value*>             iterator;
  typedef __rb_tree_iterator<Value, const Value&, const Value*> const_iterator;
  typedef __rb_tree_iterator<Value, Ref, Ptr>                   self;
  typedef __rb_tree_node<Value>* link_type; //指向RB-tree节点的指针类型

  __rb_tree_iterator() {}
  __rb_tree_iterator(link_type x) { node = x; }
  __rb_tree_iterator(const iterator& it) { node = it.node; }

  //解引用操作为获取所指RB-tree节点的value
  reference operator*() const { return link_type(node)->value_field; }
#ifndef __SGI_STL_NO_ARROW_OPERATOR
  pointer operator->() const { return &(operator*()); }
#endif /* __SGI_STL_NO_ARROW_OPERATOR */

  //调用父类的increment()，函数会修改node成员，使其指向后一个RB-tree节点
  self& operator++() { increment(); return *this; }
  self operator++(int) {
    self tmp = *this;
    increment();
    return tmp;
  }
    
  //调用父类的decrement()，函数会修改node成员，使其指向前一个RB-tree节点
  self& operator--() { decrement(); return *this; }
  self operator--(int) {
    self tmp = *this;
    decrement();
    return tmp;
  }
};
```

### 5.2.5 RB-tree的数据结构

```c++
template <class Key, class Value, class KeyOfValue, class Compare,
          class Alloc = alloc>
class rb_tree {
protected:
  typedef void* void_pointer;
  typedef __rb_tree_node_base* base_ptr;
  typedef __rb_tree_node<Value> rb_tree_node;
  typedef simple_alloc<rb_tree_node, Alloc> rb_tree_node_allocator;
  typedef __rb_tree_color_type color_type;
public:
  typedef Key key_type;
  typedef Value value_type;
  typedef value_type* pointer;
  typedef const value_type* const_pointer;
  typedef value_type& reference;
  typedef const value_type& const_reference;
  typedef rb_tree_node* link_type;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;
  //...
protected:
  size_type node_count; // keeps track of size of tree
  link_type header;  
  Compare key_compare;
  //...
public:
  typedef __rb_tree_iterator<value_type, reference, pointer> iterator;
  //...
};
```

### 5.2.6 RB-tree的构造

> RB-tree的空间配置器

```c++
class rb_tree {
protected:
  //...
  typedef __rb_tree_node<Value> rb_tree_node;
  typedef simple_alloc<rb_tree_node, Alloc> rb_tree_node_allocator;
  //...
};
```

> RB-tree的构造

RB-tree的构造方式有两种：

- 1.现有的RB-tree复制一个新的RB-tree
- 2.产生一颗空树

**产生空树**：

```c++
/*
template <class Key, class Value, class KeyOfValue, class Compare,
          class Alloc = alloc>

Key、Value、KeyOfValue、Compare、Alloc(这里使用默认alloc）
*/
rb_tree<int,int,identity<int>,less<int>> itree;
```

**rb_tree()**：

```c++
class rb_tree {
  //...
  void init() {
    //产生一个节点空间，令header指向它
    header = get_node(); 
    //令header为红色，用于区分header与root（root为黑色）
    color(header) = __rb_tree_red; 
                                   
    root() = 0;
    //令header的左子节点为自己
    leftmost() = header;
    //令header的右子节点为自己
    rightmost() = header;
  }
public:
  
  rb_tree(const Compare& comp = Compare())
    : node_count(0), key_compare(comp) { init(); }
  //...
};
```



### 5.2.7 RB-tree的元素操作

- **节点操作**
  - [涉及内存管理的操作](STL/RB-tree-涉及内存管理的操作.md)
    - 分配节点：get_node()
    - 释放节点：put_node()
    - 创建节点：create_node()
    - 拷贝节点：clone_node()
    - 销毁节点：destroy_node()
  - [获取节点成员的操作](STL/RB-tree-获取节点成员的操作.md)
    - left()
    - right()
    - parent()
    - value()
    - key()
    - color()
- **[RB-tree元素操作](STL/RB-tree-元素操作.md)**
  - 获取root节点：root()
  - 获取最左子节点：leftmost()
  - 获取最右子节点：rightmost()
  - 获取起始节点：begin()
  - 获取末尾节点：end()
  - 是否为空：empty()
  - 大小：size()
  - 可容纳的最大值：max_size()
  - **[插入节点](STL/RB-tree-插入节点.md)**：
    - 节点值独一无二：insert_unique()
    - 允许节点值重复：insert_equal()
  - **[元素搜索](STL/RB-tree-元素搜索.md)**： 
    - find()

## 5.3 set

**特性**：

- 1.所有元素都会根据元素的键值自动被排序
- 2.不同时拥有实值和键值，键值就是实值，实值就是键值
- 3.set不允许两个元素有相同的键值（multiset允许）
- 4.迭代器不能改变set的元素值（因为需要根据元素值排序），因此set的迭代器是一种constant iterators
- 5.当客户端对它进行元素新增操作或删除操作时，操作之前的所有迭代器，在操作完之后都依然有效（被删除的那个元素的迭代器除外）

**底层机制**：标准STL set以RB-tree为底层机制，几乎所有的set操作行为，都只是转调用RB-tree的操作行为而已

**set定义:**

```c++
template <class Key, class Compare = less<Key>, class Alloc = alloc>
class set {
public:
    ...
    //键值和实值类型相同，比较函数也是同一个
    typedef Key key_type;
    typedef Key value_type;
    typedef Compare key_compare;
    typedef Compare value_compare;
private:
    /*以下的identity为RB-tree中使用的KeyofValue，用以获取key
    identity定义于<stl_function.h>
    
    template <class T>
    struct identity : public unary_function<T, T> {
       const T& operator()(const T& x) const { return x; }
     };
    */
    typedef rb_tree<key_type, value_type, 
                  identity<value_type>, key_compare, Alloc> rep_type;
    rep_type t;  // 内含一棵RB-tree，使用RB-tree来表现set
public:
    //...
    //iterator定义为RB-tree的const_iterator，表示set的迭代器无法执行写操作
    typedef typename rep_type::const_iterator iterator;
    //...
};
```

**[set相关操作](STL/set-相关操作.md)**

- 所有的set的操作行为，RB-tree都已提供，set只是转调用
- 插入操作调用的是RB-tree的insert_unique()

## 5.4 map

**特性**：

- 1.所有元素都会根据元素的键值自动排序
- 2.map的所有元素都是pair，同时拥有实值和键值
- 3.map不允许两个元素拥有相同的键值
- 4.不可以改变元素的键值（因为根据键值进行排序），可以改变元素的实值，因此map的迭代器既不是constant iterators，也不是mutable iterators
- 5.当客户端对它进行元素新增操作或删除操作时，操作之前的所有迭代器，在操作完之后都依然有效（被删除的那个元素的迭代器除外）

**底层机制**：标准STL map以RB-tree为底层机制，几乎所有的map操作行为，都只是转调用RB-tree的操作行为而已

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-20.png)

**pair的定义**：

```c++
//<stl_pair.h>
template <class T1, class T2>
struct pair {
  typedef T1 first_type;
  typedef T2 second_type;

  T1 first; //注意，它是public
  T2 second; //注意，它是public
  pair() : first(T1()), second(T2()) {}
  pair(const T1& a, const T2& b) : first(a), second(b) {}
};
```

**map定义**：

```c++
template <class Key, class T, class Compare = less<Key>, class Alloc = alloc>
class map {
public:
  typedef Key key_type;     //键值类型
  typedef T data_type;      //实值类型
  typedef T mapped_type;    
  typedef pair<const Key, T> value_type;    //键值对，RB-tree节点中的value类型
  typedef Compare key_compare;  //键值比较函数

  //...

private:
  /*
  select1st<value_type>是RB-tree中使用的KeyofValue
  */
  typedef rb_tree<key_type, value_type, 
                  select1st<value_type>, key_compare, Alloc> rep_type;
  rep_type t;  // 内含一棵RB-tree，使用RB-tree来表现map
public:
  //...
  //迭代器和set不同，允许修改实值
  typedef typename rep_type::iterator iterator;
  ...

  //下标操作
  T& operator[](const key_type& k) {
    return (*((insert(value_type(k, T()))).first)).second;
  }

  //插入操作，使用的是RB-tree的insert_unique()
  pair<iterator,bool> insert(const value_type& x) { return t.insert_unique(x); }

  //...
};
```

**[map相关操作](STL/map-相关操作.md)**

- 所有的map的操作行为，RB-tree都已提供，map只是转调用
- 插入操作调用的是RB-tree的insert_unique()

**insert()**：

- 返回值是一个pair，由一个迭代器和一个bool值组成，后者表示插入成功与否，成功的话前者即指向被插入的那个元素

```c++
  pair<iterator,bool> insert(const value_type& x) { return t.insert_unique(x); }
```

**operator[]**：

用法有两种：

- 左值：`simap[string("jjhou")] = 1;`
- 右值：`int number = simap[string("jjhou")];`

```c++
  T& operator[](const key_type& k) {
    return (*((insert(value_type(k, T()))).first)).second;
  }

//分解为：
//1.根据键值和实值做出一个元素，
//由于实值未知，所以T()产生一个与实值类型相同的临时对象代替，
value_type(k, T())
//2.把这个元素插入到map里面去，
//返回一个pair，第一个元素指向插入妥当的新元素，
//或指向插入失败点（键值重复）的旧元素
//如果作为左值，以此“实值待填”的元素将位置卡好
//如果作为右值，此时的插入操作返回的pair的第一个元素（迭代器）指向键值符合的旧元素
insert(value_type(k, T()))
//3.取出pair的第一个元素，这是一个迭代器，指向被插入的元素
(insert(value_type(k, T()))).first
//4.解引用迭代器，获得一个map元素，是一个由键值和实值组成的pair
*(insert(value_type(k, T()))).first
//5.取map元素pair的第二个元素
(*(insert(value_type(k, T()))).first).second
//6.最后以T& 引用传递方式传回
```

## 5.5 multiset

multiset的特性及用法和set完全相同，唯一的差别在于它允许键值重复，插入操作采用的是底层机制RB-tree的insert_equal()而非insert_unique() 

## 5.6 multimap

multimap的特性及用法和map完全相同，唯一的差别在于它允许键值重复，插入操作采用的是底层机制RB-tree的insert_equal()而非insert_unique() 

## 5.7 hashtable

**二叉搜索树特点**：具有“对数平均时间”（O(logN)）的表现，**但**输入数据需要有足够的随机性

**散列表特点**：具有“常数平均时间”（O(N)）的表现，以统计为基础，不需要依赖输入元素的随机性

**散列函数**：负责将一个元素映射为一个“大小可接收的索引”

- 存在**碰撞问题**：因为元素个数大于array容量，可能有不同的元素被映射到相同的位置（亦即有相同的索引）
- **解决方法**：线性探测、二次探测、开链等

**负载系数**：元素个数除以表格大小，负载系数永远在0~1之间（除非采用**开链策略**）

> 线性探测

**插入**：计算出某个元素的插入位置，而该位置上的空间不再可用时，循序往下一一寻找

**搜索**：计算出来的位置上的元素与我们搜索的目标不符，就循序往下一一寻找，直到找到吻合者，或遇到空格元素

**删除**：采用惰性删除，即只标记删除记号，实际删除操作则待表格重新整理（因为搜索时的循序查找过程）

线性探测存在**基本聚集**（primary clustering）：多个元素具有相同的索引（或在一块区域），那么这些元素插入时需要循序往下一一寻找合适的位置。平均插入成本的成长幅度远高于负载系数的成长幅度。基本聚集会产生很长的探查序列，从而使表变得越来越大。这种过度的探查会降低表的性能

- 解决方法：二次探测

> 二次探测

如果散列函数计算出新元素的位置为H，而该位置实际上已经被使用，那么我们就依序尝试`H+1^2`、`H+2^2`、`H+3^2`、`H+4^2`、...`H+i^2`，而不是像线性探测那样依次尝试`H+1`、`H+2`、`H+3`、`H+4`...`H+i`

二次探测解决了**基本聚集**，但存在**二次聚集**（secondary clustering）：两个元素经散列函数计算出的位置若相同，则插入时所探测的位置也相同，形成某种浪费

- 解决方法：双散列法（double hashing）

> 开链

**做法**：在每一个表格元素中维护一个list。散列函数为我们分配一个list，然后在该list身上执行元素的插入、搜索、删除等操作。针对list进行的搜索时一种线性操作

表格的负载系数大于1

SGI STL的hash table采用的是开链法

### 5.7.2 hashtable的桶（buckets）与节点（nodes）

SGI STL的hash table采用的是开链法

遵循SGI的命名，称hash table表格内的元素位桶（buckets），表格内的每个单元，涵盖的是节点（一个或多个）

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-24.png)

**hash bale的节点定义**：

bucket所维护的linked list不是STL的list或slist，而是自行维护的hash table node

```c++
template <class Value>
struct __hashtable_node
{
    __hashtable_node *next;
    Value val;
};
```

### 5.7.3 hashtable的迭代器

```c++
template <class Value, class Key, class HashFcn,
          class ExtractKey, class EqualKey, class Alloc>
struct __hashtable_iterator {
  typedef hashtable<Value, Key, HashFcn, ExtractKey, EqualKey, Alloc>
          hashtable;
  typedef __hashtable_iterator<Value, Key, HashFcn, 
                               ExtractKey, EqualKey, Alloc>
          iterator;
  typedef __hashtable_const_iterator<Value, Key, HashFcn, 
                                     ExtractKey, EqualKey, Alloc>
          const_iterator;
  typedef __hashtable_node<Value> node;

  typedef forward_iterator_tag iterator_category;
  typedef Value value_type;
  typedef ptrdiff_t difference_type;
  typedef size_t size_type;
  typedef Value& reference;
  typedef Value* pointer;

  node* cur;        //迭代器目前所指的节点
  hashtable* ht;    //指向相应的hashtable（因为可能需要从一个bucket跳到下一个bucket）

  __hashtable_iterator(node* n, hashtable* tab) : cur(n), ht(tab) {}
  __hashtable_iterator() {}
  reference operator*() const { return cur->val; }
  pointer operator->() const { return &(operator*()); }
  iterator& operator++();
  iterator operator++(int);
  bool operator==(const iterator& it) const { return cur == it.cur; }
  bool operator!=(const iterator& it) const { return cur != it.cur; }
};
```

bashtable迭代器必须维系着整个“bucket vector”的关系，并记录着所指的节点

**前进操作**

- 首先尝试从目前所指向的节点出发，前进一个节点（利用节点的next即可轻易完成）
- 如果目前节点正巧是list的尾端，就调至下一个bucket身上，指向下一个bucket的list的头部节点

```c++
template <class V, class K, class HF, class ExK, class EqK, class A>
__hashtable_iterator<V, K, HF, ExK, EqK, A>&
__hashtable_iterator<V, K, HF, ExK, EqK, A>::operator++()
{
  const node* old = cur;
  cur = cur->next;  //如果存在，就是它。否则进入以下if流程
  if (!cur) {
    //根据元素值，定位出下一个bucket，其起头处就是我们的目的地
    size_type bucket = ht->bkt_num(old->val);
    while (!cur && ++bucket < ht->buckets.size())
      cur = ht->buckets[bucket];
  }
  return *this;
}

template <class V, class K, class HF, class ExK, class EqK, class A>
inline __hashtable_iterator<V, K, HF, ExK, EqK, A>
__hashtable_iterator<V, K, HF, ExK, EqK, A>::operator++(int)
{
  iterator tmp = *this;
  ++*this;
  return tmp;
}
```

hashtable的迭代器没有后退操作，hashtable也没有定义所谓的逆向迭代器 

### 5.7.4 hashtable的数据结构

buckets聚合体以vector完成

```c++
template <class Value, class Key, class HashFcn,
          class ExtractKey, class EqualKey, class Alloc = alloc>
class hashtable;

...

template <class Value, class Key, class HashFcn,
          class ExtractKey, class EqualKey,
          class Alloc> //先前声明时，已给出Alloc默认值alloc
class hashtable {
public:
  typedef HashFcn hasher;
  typedef EqualKey key_equal;
  ...
private:
  //以下3者都是function  objects
  hasher hash;
  key_equal equals;
  ExtractKey get_key;

  typedef __hashtable_node<Value> node;  //hashtable节点类型
  typedef simple_alloc<node, Alloc> node_allocator;

  vector<node*,Alloc> buckets; //hashtable的桶数组，以vector完成
  size_type num_elements;      //元素个数
  ...
};
```

对模板参数的解释：

- Value：节点的实值类型
- Key：节点的键值类型
- HashFcn：散列函数的函数类型
- ExtractKey：从节点中取出键值的方法（函数或仿函数）
- EqualKey：判断键值相同与否的方法（函数或仿函数）
- Alloc：空间配置器，缺省使用std::alloc

<stl_hash_fun.h>中定义了数个现场的散列函数（仿函数）

散列函数计算bucket的位置，SGI将这项任务赋予bkt_num()，由它调用散列函数取得一个可以执行取模运算的值

SGI STL以质数来设计表格大小，并且先将28个质数（逐渐呈现大约2倍的关系）计算好，以备随时访问，同时提供一个函数，用来查询在这28个质数中，“最接近某数并大于某数”的质数： 

```c++
//注意，long至少有32位
static const int __stl_num_primes = 28;
static const unsigned long __stl_prime_list[__stl_num_primes] =
{
  53,         97,           193,         389,       769,
  1543,       3079,         6151,        12289,     24593,
  49157,      98317,        196613,      393241,    786433,
  1572869,    3145739,      6291469,     12582917,  25165843,
  50331653,   100663319,    201326611,   402653189, 805306457, 
  1610612741, 3221225473ul, 4294967291ul
};

//该函数被next_size()所调用
//以下函数找出上述28个质数中，最接近并大于或等于n的那个质数
inline unsigned long __stl_next_prime(unsigned long n)
{
  const unsigned long* first = __stl_prime_list;
  const unsigned long* last = __stl_prime_list + __stl_num_primes;
  const unsigned long* pos = lower_bound(first, last, n);
  return pos == last ? *(last - 1) : *pos;
}

//总共可以有多少buckets，返回值为4294967291
size_type max_bucket_count() const
  { return __stl_prime_list[__stl_num_primes - 1]; } 
```

### 5.7.5 hashtable的构造

> 配置节点和释放节点

- new_node()
- delete_node()

```c++
  node* new_node(const value_type& obj)
  {
    node* n = node_allocator::allocate();
    n->next = 0;
    __STL_TRY {
      construct(&n->val, obj);
      return n;
    }
    __STL_UNWIND(node_allocator::deallocate(n));
  }

  void delete_node(node* n)
  {
    destroy(&n->val);
    node_allocator::deallocate(n);
  }
```

> 构造函数

**使用例子**：

对模板参数的解释：

- Value：节点的实值类型
- Key：节点的键值类型
- HashFcn：散列函数的函数类型
- ExtractKey：从节点中取出键值的方法（函数或仿函数）
- EqualKey：判断键值相同与否的方法（函数或仿函数）
- Alloc：空间配置器，缺省使用std::alloc

```c++
//
hashtable<int, int, hash<int>, identity<int>, equal_to<int>, alloc> iht(50, hash<int>(), equal_to<int>());
```

**调用过程**：

```
hashtable()
	---> initialize_buckets()
		---> next_size()
```

**代码**：

```c++
  hashtable(size_type n,
            const HashFcn&    hf,
            const EqualKey&   eql)
    : hash(hf), equals(eql), get_key(ExtractKey()), num_elements(0)
  {
    initialize_buckets(n);
  }

  void initialize_buckets(size_type n)
  {
    //传入50，返回53
    const size_type n_buckets = next_size(n);
    //保留53个元素空间
    buckets.reserve(n_buckets);
    //将53个元素空间都设为nullptr（因为没有元素）
    buckets.insert(buckets.end(), n_buckets, (node*) 0);
    num_elements = 0;
  }
  //返回最接近n并大于或等于n的质数
  size_type next_size(size_type n) const { return __stl_next_prime(n); 
```

### 5.7.6 hashtable的元素操作

- 重建表操作：resize()
- 不允许重复插入：insert_unique()
- 允许重复插入：insert_equal()
- 判断bucket位置：bkt_num()
- 整体删除：clear()
- 复制：copy_from()
- 查找元素：find()
- 统计某键值的元素个数：count()

> **[重建表操作 resize()](STL/hashtable-resize().md)**

**表格重建的判断原则**：拿元素个数（把新增元素计入后）和bucket vector的大小比较，如果前者大于后者，则重建表格。因此可知，list的最大容量与vector的大小相同

**操作**：

- 1.处理每一个旧的bucket
- 2.指向bucket中list的起始节点
- 3.找到节点对应新的bucket号
- 4.令目前处理的旧bucket起始节点为first的下一个节点
- 5.将当前节点链入新的bucket的list中（放于list头）
- 6.继续处理旧bucket的下一个节点

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-5-25.png)

> **[不允许重复插入 insert_unique()](STL/hashtable-insert_unique().md)**

**使用例子**：

```c++
iht.insert_unique(59);
iht.insert_unique(60);
```

```c++
insert_unique()
    ---> resize()  //判断是否需要重建表格，如果需要就扩充
    ---> insert_unique_noresize()  //1.计算应位于哪个bucket
                                   //2.遍历到节点串查找是否存在键值重复的节点,如果有则什么都不做，返回pair，第一个元素指向重复的元素的迭代器，第二个元素为false
                                   //3.将新节点插入list头部
                                   //4.返回插入节点的迭代器和成功组成的pair
```

> **[允许重复插入 insert_equal()](STL/hashtable-insert_equal().md)**

**使用例子**：

```c++
ith.insert_equal(59);
ith.insert_equal(59);
```

```c++
insert_equal()
    ---> resize()  //判断是否需要重建表格，如果需要就扩充
    ---> insert_equal_noresize(obj)  //1.如果找到键值相同的节点，直接构建新节点，并将其插入到键值相同的节点后，返回
                                     //2.如果没找到键值相同的节点，则在节点串的首部插入新节点
```

> **判断bucket位置 bkt_num()**

```c++
  //版本1：接受实值和bucket个数
  size_type bkt_num(const value_type& obj, size_t n) const 
  {
    return bkt_num_key(get_key(obj), n); //调用版本4
  }
  //版本2：只接受实值
  size_type bkt_num(const value_type& obj) const 
  {
    return bkt_num_key(get_key(obj)); //调用版本3
  }

  //版本3：只接受键值
  size_type bkt_num_key(const key_type& key) const //版本3
  {
    return bkt_num_key(key, buckets.size()); //调用版本4
  }
  //版本1：接受键值和bucket个数
  size_type bkt_num_key(const key_type& key, size_t n) const //版本4
  {
    return hash(key) % n; //返回key和buckets数目取模的结果
  }
```

> **整体删除 clear()**

**步骤**：

- 1.删除list中每个节点
- 2.将bucket置空

```c++
template <class V, class K, class HF, class Ex, class Eq, class A>
void hashtable<V, K, HF, Ex, Eq, A>::clear()
{
  //遍历每一个bucket
  for (size_type i = 0; i < buckets.size(); ++i) {
    node* cur = buckets[i];
    while (cur != 0) {
      node* next = cur->next;
      delete_node(cur); //释放当前节点
      cur = next;       //继续处理下一节点
    }
    buckets[i] = 0; //将bucket的节点串设为空
  }
  num_elements = 0; //将hashtable的元素个数设为0
}
```

> **复制 copy_from()**

**步骤**：

- 1.清除自己的节点
- 2.根据ht的bucket数量为自己预留空间。如果本身空间已经足够就不进行处理，否则会增大
- 3.从自己的buckets vector尾端开始，插入n个源，值为Null（此时bucket vector为空，所以所谓的尾端，就是起始处）
- 4.遍历hashtable的每一个bucket，复制每一个bucket中的节点

```c++
template <class V, class K, class HF, class Ex, class Eq, class A>
void hashtable<V, K, HF, Ex, Eq, A>::copy_from(const hashtable& ht)
{
  //先清除自己的节点
  buckets.clear();
  //根据ht的bucket数量为自己预留空间
  //如果本身空间已经足够就不进行处理，否则会增大
  buckets.reserve(ht.buckets.size());
  //从自己的buckets vector尾端开始，插入n个源，值为Null
  //此时bucket vector为空，所以所谓的尾端，就是起始处
  buckets.insert(buckets.end(), ht.buckets.size(), (node*) 0);
  __STL_TRY {
    //遍历bt的每一个bucket，复制每一个bucket中的节点
    for (size_type i = 0; i < ht.buckets.size(); ++i) {
      if (const node* cur = ht.buckets[i]) {
        node* copy = new_node(cur->val);
        buckets[i] = copy;
       
        //针对同一个list，复制每一个节点
        for (node* next = cur->next; next; cur = next, next = cur->next)        {
          copy->next = new_node(next->val);
          copy = copy->next;
        }
      }
    }
    num_elements = ht.num_elements; //更新节点数
  }
  __STL_UNWIND(clear());
}
```

> **查找元素 find()**

```c++
  iterator find(const key_type& key) 
  {
    //首先寻找落在哪一个bucket内
    size_type n = bkt_num_key(key);
    node* first;
    //从bucket list的头开始，一一对比每个元素的键值，对比成功就跳出
    for ( first = buckets[n];
          first && !equals(get_key(first->val), key);
          first = first->next)
      {}
    return iterator(first, this);
} 
```

> **统计某键值的元素个数 count()**

```c++
  size_type count(const key_type& key) const
  {
    const size_type n = bkt_num_key(key);
    size_type result = 0;
    //从bucket list的头开始，一一对比每个元素的键值，对比成功就加1
    for (const node* cur = buckets[n]; cur; cur = cur->next)
      if (equals(get_key(cur->val), key))
        ++result;
    return result;
}
```

### 5.7.7 hash function

<stl_hash_fun.h>定义数个现成的hash function，全是仿函数

hash function是计算元素位置的函数，SGI将这项任务赋予先前提到过的bkt_num()，再由它来调用这里提供的hash function，取得一个可以对hash table进行模运算的值。针对char、int、long等整数类型，大部分的hash functions什么也没做，只是忠实返回原值。对于字符串(const char*)类型，设计了一个转换函数

**对于各种类型的hash function**：

- char：大部分的hash functions什么也没做，只是忠实返回原值
- int：大部分的hash functions什么也没做，只是忠实返回原值
- long：大部分的hash functions什么也没做，只是忠实返回原值
- (const char*)：__stl_hash_string处理
- string：无法处理，用户必须自行为它们定义hash function
- double：无法处理，用户必须自行为它们定义hash function
- float：无法处理，用户必须自行为它们定义hash function

```c++
inline size_t __stl_hash_string(const char* s)
{
  unsigned long h = 0; 
  for ( ; *s; ++s)
    h = 5*h + *s;
  
  return size_t(h);
}

//以下所有的__STL_TEMPLATE_NULL定义为template<>
__STL_TEMPLATE_NULL struct hash<char*>
{
  size_t operator()(const char* s) const { return __stl_hash_string(s); }
};

__STL_TEMPLATE_NULL struct hash<const char*>
{
  size_t operator()(const char* s) const { return __stl_hash_string(s); }
};

__STL_TEMPLATE_NULL struct hash<char> {
  size_t operator()(char x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<unsigned char> {
  size_t operator()(unsigned char x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<signed char> {
  size_t operator()(unsigned char x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<short> {
  size_t operator()(short x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<unsigned short> {
  size_t operator()(unsigned short x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<int> {
  size_t operator()(int x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<unsigned int> {
  size_t operator()(unsigned int x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<long> {
  size_t operator()(long x) const { return x; }
};
__STL_TEMPLATE_NULL struct hash<unsigned long> {
  size_t operator()(unsigned long x) const { return x; }
};
```

**直接以SGI hashtable处理string的错误现象**：

```c++
#include <hash_set>
#include <iostream>
#include <string>
using namespace std;

int main()
{
    //hash<string>无定义
    hashtable<string,string,hash<string>,identity<string>,equal_to<string>
        ,alloc> iht(50,hash<string>(),equal_to<string>());
    
    iht.size(); //0
    iht.bucket_count(); //53
    iht.insert_unique(string("jjhou")); //error
    
    //hashtable无法处理的类型，hash_set当然也无法处理
    hash_set<string> shs;
    hash_set<double> dhs;
    
    shs.insert(string("jjhou")); //error
    dhs.insert(15.0); //error
}
```

## 5.8 hash_set

hash_set以hashtable为底层机制，由于hash_set所供应的操作接口hashtable都提供了，所以几乎所有的hash_set操作行为，都只是转调用hashtable的操作行为而已 

**set与hash_set的区别**：

- **自动排序**：RB-tree有自动排序功能而hashtable没有，因此set的元素有自动排序而hash_set没有
- **类型支持**：hashtable有一些无法处理的类型（除非用户为那些类别编写hash function），凡是hashtable无法处理的，hash_set也无法处理

## 5.9 hash_map

hash_map以hashtable为底层机制，由于hash_map所供应的操作接口hashtable都提供了，所以几乎所有的hash_map操作行为，都只是转调用hashtable的操作行为而已 

**map与hash_map的区别**：

- **自动排序**：RB-tree有自动排序功能而hashtable没有，因此map的元素有自动排序而hash_map没有
- **类型支持**：hashtable有一些无法处理的类型（除非用户为那些类别编写hash function），凡是hashtable无法处理的，hash_map也无法处理

## 5.10 hash_multiset

hash_multiset和hash_set实现上的唯一差别在于，前者的元素插入操作采用底层机制hashtable的insert_equal()，后者则是采用insert_unique() 

## 5.11 hash_multimap

hash_multimap和hash_map实现上的唯一差别在于，前者的元素插入操作采用底层机制hashtable的insert_equal()，后者则是采用insert_unique() 

# 第6章 算法

所有的STL算法都作用在由迭代器[first,last)所标出来的区间上

**质变算法**（mutating algorithms）：运算过程中会更改区间内（迭代器所指）的元素的内容

**非质变算法**（nomutating algorithms）：运算过程中不会改变区间内（迭代器所指）的元素内容

表格中凡是不在STL标准规格之列的SGI专属算法，都以*加以标示

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-%E7%AE%97%E6%B3%95%E6%80%BB%E8%A7%881.png)
![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-%E7%AE%97%E6%B3%95%E6%80%BB%E8%A7%882.png)
![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-%E7%AE%97%E6%B3%95%E6%80%BB%E8%A7%883.png)
![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-%E7%AE%97%E6%B3%95%E6%80%BB%E8%A7%884.png)

### 6.1.5 STL算法的一般形式

**函数缺省行为**：

许多STL算法不只支持一个版本：这一类算法的某个版本采用缺省运算行为，另一个版本提供额外参数，接受外界传入一个仿函数，以便采用其他策略。有两种形式：

- 重载函数：例如unique()
- 不同名称的函数：使用非缺省行为的版本一般以`_if`结尾，如`find_if()`

**质变算法的原地算法**：

- in-place（就地进行）版：就地改变操作对象
- copy（另地进行）版：将操作对象的内容复制一份副本，然后再副本上进行修改并返回该副本。copy版总是以`_copy`结尾，如`replace()_copy()`

但并不是所有质变算法都有copy版本，例如sort()就没有，如果希望“无copy版本”的算法实施于某一段区间元素的副本身上，调用函数前需自行制作副本，并以副本调用函数

**算法位置**

- 数值算法：实现于SGI的`<stl_numeric.h>`，是内部文件。STL规定用户必须包含的是上层的`<numeric>`
- 其他算法：实现于SGI的`<stl_algobase.h>`（基本算法）和`<stl_algo.h>`（其他算法），是内部文件。STL规定用户必须包含的是上层的`<algorithm>`

## 6.3 数值算法 <stl_numeric.h>

### 6.3.2 accumulate

accumulate用来计算init和[first,last)内所有元素的和

**例子**：

```c++
int ia[5] = { 1, 2, 3, 4, 5 };
vector<int> iv(ia, ia+5);

//结果为15，即0+1+2+3+4+5=15
accumulate(iv.begin(), iv.end(), 0);

//结果为-15，即0-1-2-3-4-5=-15
accumulate(iv.begin(), iv.end(), 0, minus<int>());
```

**源码分析**：

```c++
template <class InputIterator, class T>
T accumulate(InputIterator first, InputIterator last, T init) {
  for ( ; first != last; ++first)
    init = init + *first;
  return init;
}

template <class InputIterator, class T, class BinaryOperation>
T accumulate(InputIterator first, InputIterator last, T init,
             BinaryOperation binary_op) {
  for ( ; first != last; ++first)
    init = binary_op(init, *first);
  return init;
}
```

## 6.4 基本算法

定义于`<stl_algobase.h>`

### 6.4.1 equal()

如果两个序列在[first,last)区间内相等，equal返回true

前提：两个序列个数相等

```c++
template <class InputIterator1, class InputIterator2>
inline bool equal(InputIterator1 first1, InputIterator1 last1,
		  InputIterator2 first2) {
  for ( ; first1 != last1; ++first1, ++first2)
    if (*first1 != *first2)
      return false;
  return true;
}

template <class InputIterator1, class InputIterator2, class BinaryPredicate>
inline bool equal(InputIterator1 first1, InputIterator1 last1,
		  InputIterator2 first2, BinaryPredicate binary_pred) {
  for ( ; first1 != last1; ++first1, ++first2)
    if (!binary_pred(*first1, *first2))
      return false;
  return true;
}
```

### 6.4.2 fill()

将[first,last)内的所有元素改填新值

```c++
template <class ForwardIterator, class T>
void fill(ForwardIterator first, ForwardIterator last, const T& value) {
  for ( ; first != last; ++first)
    *first = value;
}
```

### 6.4.3 fill_n()

将[first,last)内的前n个元素改填新值，返回的迭代器指向被填入的最后一个元素的下一个位置

```c++
template <class OutputIterator, class Size, class T>
OutputIterator fill_n(OutputIterator first, Size n, const T& value) {
  for ( ; n > 0; --n, ++first)
    *first = value;
  return first;
}
```

### 6.4.4 iter_swap()

将两个ForwardIterator所指向的对象对调

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-iter_swap.png)

```c++
template <class ForwardIterator1, class ForwardIterator2>
inline void iter_swap(ForwardIterator1 a, ForwardIterator2 b) {
  __iter_swap(a, b, value_type(a));
}

template <class ForwardIterator1, class ForwardIterator2, class T>
inline void __iter_swap(ForwardIterator1 a, ForwardIterator2 b, T*) {
  T tmp = *a;
  *a = *b;
  *b = tmp;
}

template <class Iterator>
inline typename iterator_traits<Iterator>::value_type*
value_type(const Iterator&) {
  return static_cast<typename iterator_traits<Iterator>::value_type*>(0);
}
```

### 6.4.5 max()

取两个对象中的较大值

```c++
template <class T>
inline const T& max(const T& a, const T& b) {
  return  a < b ? b : a;
}

template <class T, class Compare>
inline const T& max(const T& a, const T& b, Compare comp) {
  return comp(a, b) ? b : a;
}
```

### 6.4.6min()

取两个对象中的较小值

```c++
template <class T>
inline const T& min(const T& a, const T& b) {
  return b < a ? b : a;
}

template <class T, class Compare>
inline const T& min(const T& a, const T& b, Compare comp) {
  return comp(b, a) ? b : a;
}
```

### 6.4.7 swap()

交换两个对象的内容

```c++
template <class T>
inline void swap(T& a, T& b) {
  T tmp = a;
  a = b;
  b = tmp;
}
```

### 6.4.8 copy()

SGI STL的copy算法用尽各种办法，包括函数重载、类型特性、偏特化等编程技巧来尽可能地加强效率

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-2.png)

copy将输入区间`[first,last)`内的元素复制到输出区间`[result,result+(last-first))`内，也就是说，它会执行赋值操作`*result = *first,*(result+1) = *(first+1),...`依次类推。返回一个迭代器：`result+(last-first)`。copy对其template参数所要求的条件非常宽松。其输入区间只需由inputIterators构成即可，输出区间只需要由OutputIterator构成即可。这**意味着可以使用copy算法，将任何容器的任何一段区间的内容，复制到任何容器的任何一段区间上** 

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-3.png)

**重叠问题**：如果输出区间的起点位于输入区间内，copy算法便**可能**会在输入区间的（某些）元素尚未被复制之前，就覆盖其值，导致错误。如果copy算法的迭代器决定调用memmove()来执行任务，就不会造成上述错误。因为memmove()会将整个输入区间的内容复制下来，没有被覆盖的危险

**空容器copy问题**：copy会为输出区间内的元素赋予新值，而不是产生新的元素。它不能改变输出区间的迭代器个数。换句话说，copy不能直接用来将元素插入空容器中。如果想将元素插入序列之内，要么使用序列容器的insert成员函数，要么使用copy算法并搭配insert_iterator 

> **copy()函数**

有三个版本：

- 1.完全泛化版本
  - **__copy_dispatch()**
- 2.针对原生指针const char*的特化版本
  - **memmove()**
- 3.针对原生指针const wchar_t*的特化版本
  - **memmove()**

```c++
//1.完全泛化版本
template <class InputIterator, class OutputIterator>
inline OutputIterator copy(InputIterator first, InputIterator last,
                           OutputIterator result)
{
  return __copy_dispatch<InputIterator,OutputIterator>()(first, last, result);
}

//针对原生指针const char*的特化版本（memmove）
inline char* copy(const char* first, const char* last, char* result) {
  memmove(result, first, last - first);
  return result + (last - first);
}

//3.针对原生指针const wchar_t*的特化版本（memmove）
inline wchar_t* copy(const wchar_t* first, const wchar_t* last,
                     wchar_t* result) {
  memmove(result, first, sizeof(wchar_t) * (last - first));
  return result + (last - first);
}
```

> **__copy_dispatch()**

有三个版本：

- 1.完全泛化版本
  - __copy(）（InputIterator版本）：以迭代器等同于否，决定循环是否继续，效率低
  - __copy(）（RandomAccessIterator版本）
    - __copy_d()：以n决定循环的次数，效率快
- 2.指针对两个参数都是`T*`指针形式的偏特化版本
  - __copy_t()(无用赋值操作符版)
    - memmove()
  - __copy_t()(有用赋值操作符版)
    - __copy_d()
- 3.针对第一个参数为`const T*`指针形式，第二个参数为`T*`指针形式的偏特化版本
  - __copy_t()(无用赋值操作符版)
    - memmove()
  - __copy_t()(有用赋值操作符版)
    - __copy_d()

```c++
//1.完全泛化版本
template <class InputIterator, class OutputIterator>
struct __copy_dispatch
{
  OutputIterator operator()(InputIterator first, InputIterator last,
                            OutputIterator result) {
    return __copy(first, last, result, iterator_category(first));
  }
};

//2.指针对两个参数都是T*指针形式的偏特化版本
template <class T>
struct __copy_dispatch<T*, T*>
{
  T* operator()(T* first, T* last, T* result) {
    typedef typename __type_traits<T>::has_trivial_assignment_operator t; 
    return __copy_t(first, last, result, t());
  }
};

//3.针对第一个参数为const T*指针形式，第二个参数为T*指针形式的偏特化版本
template <class T>
struct __copy_dispatch<const T*, T*>
{
  T* operator()(const T* first, const T* last, T* result) {
    typedef typename __type_traits<T>::has_trivial_assignment_operator t; 
    return __copy_t(first, last, result, t());
  }
};
```

> **__copy(）函数**

```c++
//InputIterator版本
template <class InputIterator, class OutputIterator>
inline OutputIterator __copy(InputIterator first, InputIterator last,
                             OutputIterator result, input_iterator_tag)
{
  //以迭代器等同于否，决定循环是否继续，效率低
  for ( ; first != last; ++result, ++first)
    *result = *first;
  return result;
}

//RandomAccessIterator版本
template <class RandomAccessIterator, class OutputIterator>
inline OutputIterator 
__copy(RandomAccessIterator first, RandomAccessIterator last,
       OutputIterator result, random_access_iterator_tag)
{
  return __copy_d(first, last, result, distance_type(first));
}
```

> **__copy_t()函数**：

```c++
//无用赋值操作符版
template <class T>
inline T* __copy_t(const T* first, const T* last, T* result, __true_type) {
  memmove(result, first, sizeof(T) * (last - first));
  return result + (last - first);
}

//有用赋值操作符版
template <class T>
inline T* __copy_t(const T* first, const T* last, T* result, __false_type) {
  return __copy_d(first, last, result, (ptrdiff_t*) 0);
}
```

> **__copy_d()函数**

```c++
template <class RandomAccessIterator, class OutputIterator, class Distance>
inline OutputIterator
__copy_d(RandomAccessIterator first, RandomAccessIterator last,
         OutputIterator result, Distance*)
{
  for (Distance n = last - first; n > 0; --n, ++result, ++first) 
    *result = *first;
  return result;
}
```

### 6.4.9 copy_backward()

copy_backward将`[first,last)`区间的每一个元素，以逆行的方向复制到以result-1为起点，方向亦为逆行的区间上。换句话说，copy_backward算法会执行赋值操作`*(result-1) = *(last - 1),*(result-2) = *(last - 2),...`以此类推，返回一个迭代器：`result-(last-first)` 

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-4.png)

## 6.5 Set相关算法

STL提供了四种与set相关的算法：并集、交集、差集、对称差集

**要求**：排序区间。因此以hashtable为底层地址的hash_set无法使用这四个算法

> **set_union()**

set_union()可构造S1、S2的并集，即`S1 ∪ S2`，此集合内含S1或S2的每一个元素

S1、S2及并集都是以排序区间表示

返回值为迭代器，指向输出区间的尾端

每个元素都不需要唯一，如果某个值在S1中出现n次，在S2中出现m次，那么该值在输出区间中会出现`max(m,n)`次

> **set_intersection()**

set_intersection()可构造S1、S2的交集，即`S1 ∩ S2`，此集合内含同时出现于S1和S2的每一个元素

S1、S2及并集都是以排序区间表示

返回值为迭代器，指向输出区间的尾端

每个元素都不需要唯一，如果某个值在S1中出现n次，在S2中出现m次，那么该值在输出区间中会出现`min(m,n)`次

> **set_difference()**

set_difference()可构造S1、S2的交集，即`S1 - S2`，此集合内含出现于S1但不出现于S2的每一个元素

S1、S2及并集都是以排序区间表示

返回值为迭代器，指向输出区间的尾端

每个元素都不需要唯一，如果某个值在S1中出现n次，在S2中出现m次，那么该值在输出区间中会出现`max(n-m,0)`次

> **set_symmetirc_difference()**

set_symmetirc_difference()可构造S1、S2的交集，即`(S1 - S2) ∪ (S2 - S1)`，此集合内含"出现于S1但不出现于S2"以及“出现于S2但不出现于S1”的每一个元素

S1、S2及并集都是以排序区间表示

返回值为迭代器，指向输出区间的尾端

每个元素都不需要唯一，如果某个值在S1中出现n次，在S2中出现m次，那么该值在输出区间中会出现`|n-m|`次

## 6.7 其他算法

定义于`<stl_algo.h>`

### 6.7.1 merge()

应用于有序区间

**作用**：将两个经过排序的集合S1和S2合并起来置于另一段空间。所得结果也是一个有序序列

**返回值**：返回一个迭代器，指向最后结果序列的最后一个元素的下一个位置

```c++
//版本1
template <class InputIterator1, class InputIterator2, class OutputIterator>
OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                     InputIterator2 first2, InputIterator2 last2,
                     OutputIterator result) {
  while (first1 != last1 && first2 != last2) {
    if (*first2 < *first1) {
      *result = *first2;
      ++first2;
    }
    else {
      *result = *first1;
      ++first1;
    }
    ++result;
  }
  //最后剩余元素以copy复制到目的端，以下两个序列一定至少有一个为空
  return copy(first2, last2, copy(first1, last1, result));
}

//版本2，使用comp进行比较
template <class InputIterator1, class InputIterator2, class OutputIterator,
          class Compare>
OutputIterator merge(InputIterator1 first1, InputIterator1 last1,
                     InputIterator2 first2, InputIterator2 last2,
                     OutputIterator result, Compare comp) {
  while (first1 != last1 && first2 != last2) {
    if (comp(*first2, *first1)) {
      *result = *first2;
      ++first2;
    }
    else {
      *result = *first1;
      ++first1;
    }
    ++result;
  }
  return copy(first2, last2, copy(first1, last1, result));
}
```

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-6c.png)

### 6.7.2 partition

partition会将区间[first,last)中的元素重新排序。所有被一元条件运算pred判定为true的元素，都会被放在区间的前段，被判定为false的元素都会被放在区间的后段。该算法不保证元素的原始相对位置。如果需要保持相对位置，调用stable_partition

```c++
template <class BidirectionalIterator, class Predicate>
BidirectionalIterator partition(BidirectionalIterator first,
                                BidirectionalIterator last, Predicate pred) {
  while (true) {
    while (true)
      if (first == last)
        return first;
      else if (pred(*first))
        ++first;
      else
        break;
    --last;
    while (true)
      if (first == last)
        return first;
      else if (!pred(*last))
        --last;
      else
        break;
    iter_swap(first, last);
    ++first;
  }
}
```

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-6d.png)

### 6.7.3 remove()

**作用**：移除[first,last)之中所有与value相等的元素，并不真正从容器中删除那些元素（容器大小并不改变），而是将每一个不与value相等的元素轮番赋值给first之后的空间

**返回值**：ForwardIterator标示出重新整理后的最后元素的下一个位置

**例子**：{0,1,0,2,0,3,0,4}-->remove(0)--->{1,2,3,4,0,3,0,4}，返回值ForwardIterator指向第5个位置

对于array不适合remove()，而应使用remove_copy()

```c++
template <class ForwardIterator, class T>
ForwardIterator remove(ForwardIterator first, ForwardIterator last,
                       const T& value) {
  first = find(first, last, value); //找到第1个相等元素
  ForwardIterator next = first;
  //原地利用remove_copy()
  return first == last ? first : remove_copy(++next, last, first, value);
}
```

### 6.7.4 remove_copy()

**作用**：移除[first,last)区间内所有与value相等的元素。不真正从容器中删除那些元素（原容器大小没有改变），而是将结果赋值到一个以result标示其实位置的容器身上（新容器与原容器可以重叠）

**返回值**：OutputIterator指出被复制的最后元素的下一个位置

```c++
template <class InputIterator, class OutputIterator, class T>
OutputIterator remove_copy(InputIterator first, InputIterator last,
                           OutputIterator result, const T& value) {
  for ( ; first != last; ++first)
    if (*first != value) {
      *result = *first;
      ++result;
    }
  return result;
}
```

### 6.7.5 replace()

**作用**：将[first,last)区间内的所有old_value都以new_value取代

```c++
template <class ForwardIterator, class T>
void replace(ForwardIterator first, ForwardIterator last, const T& old_value,
             const T& new_value) {
  for ( ; first != last; ++first)
    if (*first == old_value) *first = new_value;
}
```

### 6.7.6 reverse()

**作用**：将序列[first,last)的元素在原容器中颠倒重排

为双向迭代器和随机迭代器设计了不同版本

```c++
//分派函数
template <class BidirectionalIterator>
inline void reverse(BidirectionalIterator first, BidirectionalIterator last) {
  __reverse(first, last, iterator_category(first));
}

//双向迭代器版本
template <class BidirectionalIterator>
void __reverse(BidirectionalIterator first, BidirectionalIterator last, 
               bidirectional_iterator_tag) {
  while (true)
    if (first == last || first == --last)
      return;
    else
      iter_swap(first++, last);
}

//随机迭代器版本
template <class RandomAccessIterator>
void __reverse(RandomAccessIterator first, RandomAccessIterator last,
               random_access_iterator_tag) {
  //只有随机迭代器才能做到 first < last 判断
  while (first < last) iter_swap(first++, --last);
}
```

### 6.7.7 search()

**作用**：在序列一[first1,last1)中查找序列二[first2,last2)的首次出现点

**返回值**：如果序列一内部存在于序列二完全匹配的子序列，返回迭代器last1

```c++
template <class ForwardIterator1, class ForwardIterator2>
inline ForwardIterator1 search(ForwardIterator1 first1, ForwardIterator1 last1,
                               ForwardIterator2 first2, ForwardIterator2 last2)
{
  return __search(first1, last1, first2, last2, distance_type(first1),
                  distance_type(first2));
}

template <class ForwardIterator1, class ForwardIterator2,
          class BinaryPredicate, class Distance1, class Distance2>
ForwardIterator1 __search(ForwardIterator1 first1, ForwardIterator1 last1,
                          ForwardIterator2 first2, ForwardIterator2 last2,
                          BinaryPredicate binary_pred, Distance1*, Distance2*) {
  Distance1 d1 = 0;
  distance(first1, last1, d1);
  Distance2 d2 = 0;
  distance(first2, last2, d2);

  if (d1 < d2) return last1;

  ForwardIterator1 current1 = first1;
  ForwardIterator2 current2 = first2;

  while (current2 != last2)
    if (binary_pred(*current1, *current2)) {
      ++current1;
      ++current2;
    }
    else {
      if (d1 == d2)
        return last1;
      else {
        current1 = ++first1;
        current2 = first2;
        --d1;
      }
    }
  return first1;
}
```

### 6.7.8 sort()

**要求**：算法接受两个RandomAccessIterators

**不可用容器**

- 1.所有以RB-tree为底层结构的关系型容器（有自动排序功能，不需要）
- 2.stack、queue、priority-queue有特别的出口，不允许对元素排序
- 3.list的迭代器为BidirectionalIterators，对其排序使用其类成员函数sort
- 4.slist的迭代器为ForwordIterators，对其排序使用其类成员函数sort

**可用容器**：vector、deque

**sort的大致思想**：

- 1.数据量大时，Quick Sort，分段递归排序
- 2.在`1.`的分段递归排序中，如果**数据量**小于某个阈值，改用Insertion Sort
- 3.在`1.`的分段递归排序中，如果**递归层次**过深，改用Heap Sort

> **Insert Sort**

时间复杂度：O(N^2)

数据量很少时，效果较好（避免Quick Sort的递归调用带来过大的额外负担）

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-12.png)

> **Quick Sort**

Quick Sort是目前已知最快的排序算法，平均复杂度为O(NlogN)，最坏情况O(N^2)

**Quick Sort算法的思想**：（假设S代表被处理的序列）

- 1.如果S的元素个数为0或1，结束
- 2.取S中的任何一个元素，当作枢轴（pivot）v
- 3.将S分隔为L、R两段，使L内的每一个元素都小于或等于v，R内的每一个元素都大于或等于v
- 4.对L、R递归使用Quick Sort

**Quck Sort的精神**：将大区间分隔为小区间，分段排序。因此最坏情况发生在分割时产生出一个空的子空间，那就完全没有达到分割的预期效果

**Median-of-Three（三点中值）**：枢轴值最理想文档的方式是取整个序列的头、尾、中央三个位置的 元素，以其中值作为枢轴，这种做法称为“Median-of-Three Partition”或"Median-of-Three-QuicSort"。为了能够快速取得中央位置的元素，迭代器必须为RandomAccessIterators

**Partitioning（分割）**最有成效的方法：

- 1.令头端迭代器first向尾部移动，尾部迭代器向头部移动
- 2.当*first大于或等于枢轴时就停下来
- 3.当*last小于或等于枢轴时就停下来
- 4.检查两个迭代器是否交错
  - 不交错，将两者元素互换，然后各自调整一个位置（向中央逼近），继续相同的行为
  - 交错，表示整个序列已经调整完毕，以first为轴，分为左右两段

> **threshold（阈值）**

究竟多小的序列才应该改用Insertion Sort，并无定论，5~20可能导致差不多的结果，实际的最佳值因设备而异

> **final Insertion sort**

令某个大小以下的序列滞留在“几近排序但尚未完成”的状态，最后再以一次Insertion Sort将所有这些“几近排序但尚未竟全功”的子序列做一次完整的排序，其效率一般认为会比“将所有子序列彻底排序”更好

> **introsort**

Introspective Sorting（内省式排序），简称**introsort**

**思想**：

- 1.大部分情况与"Median-of-Three-QuicSort"完全相同
- 2.当分割行为有恶化为二次行为的倾向时，能够自我检测，转而改用Heap Sort

**效率**：效率维持在Heap Sort的O(NlogN)，又比一开始就使用Heap Sort来得好

### 6.7.9 for_each()

```c++
template <class InputIterator, class Function>
Function for_each(InputIterator first, InputIterator last, Function f) {
  for ( ; first != last; ++first)
    f(*first);
  return f;
}
```

### 6.7.10 lower_bound() 和 upper_bound()

应用于有序区间

二分查找的一种版本

**lower_bound 作用**：试图在已排序的[first,last)中寻找元素value。如果[first,last)具有与value相等的元素，便返回一个迭代器，指向其中第一个元素。如果没有这样的元素，返回“假设这样的元素存在时应该出现的位置”，即一个“不小于value”的元素

**upper_bound 作用**：试图在已经排序的[first,last)中寻找元素value。它返回“在不破坏顺序的情况下，可插入value的最后一个合适位置”

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-6-7.png)



# 第7章 仿函数 函数对象

## 7.1 仿函数概观

**仿函数**：又称函数对象，是指一种具有函数特质的对象。仿函数可以被调用者像函数一样地调用。在仿函数中以仿函数对象中所定义的`operator()`扮演函数的实质角色

**用途**：将整组操作当做算法的参数

函数指针也可以达到“将整组操作当做算法的参数”的效果，为什么还要仿函数？原因在于函数指针不能满足STL对抽象性的要求，也不能满足软件积木的要求---函数指针无法和STL其他组件（如配接器adapter）搭配，产生更灵活的变化

**使用例子**

```c++
//产生一个名为ig的对象
greater<int> ig;
//调用ig的operator()，并传入参数4和6
cout << ig(4,6);
//产生一个临时（无名）对象，并传入参数6和4
cout << greater<int>(6,4);
```

**STL仿函数与STL算法之间的关系**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-7-1.png)

任何应用程序欲使用STL的内建仿函数，都必须含入`<functional>`头文件，SGI将它们实际定义于`<stl_function.h>`

## 7.2 可配接的关键

仿函数的相应类型主要用来表现函数**参数类型**和**传回值类型**。为了方便起见，`<stl_function.h>`定义了两个类，分别代表一元仿函数和二元仿函数（STL不支持三元仿函数），其中没有任何数据成员或成员函数，只有一些类型定义。任何仿函数按照需求继承其中一个类，便自动拥有相应类型，也就拥有了配接能力

- unary_function（一元）
- binary_function（二元）

> unary_function

```c++
template <class Arg, class Result>
struct unary_function {
    typedef Arg argument_type;
    typedef Result result_type;
};
```

一旦某个仿函数继承了unary_function，其用户便可以这样取得仿函数的各种相应类型

```c++
//该仿函数继承了unary_function
template <class T>
struct negate : public unary_function<T, T> {
    T operator()(const T& x) const { return -x; }
};

//该配接器使用了某个仿函数的参数类型
template <class Predicate>
class unary_negate
  : public unary_function<typename Predicate::argument_type, bool> {
protected:
  Predicate pred;
public:
  explicit unary_negate(const Predicate& x) : pred(x) {}
  bool operator()(const typename Predicate::argument_type& x) const {
    return !pred(x);
  }
};
```

> binary_function

```c++
template <class Arg1, class Arg2, class Result>
struct binary_function {
    typedef Arg1 first_argument_type;
    typedef Arg2 second_argument_type;
    typedef Result result_type;
};    
```

一旦某个仿函数继承了binary_function，其用户便可以这样取得仿函数的各种相应类型

```c++
//该仿函数继承了binary_function
template <class T>
struct plus : public binary_function<T, T, T> {
    T operator()(const T& x, const T& y) const { return x + y; }
};

//该配接器使用了某个仿函数的参数类型和返回值类型
template <class Operation> 
class binder1st
  : public unary_function<typename Operation::second_argument_type,
                          typename Operation::result_type> {
protected:
  Operation op;
  typename Operation::first_argument_type value;
public:
  binder1st(const Operation& x,
            const typename Operation::first_argument_type& y)
      : op(x), value(y) {}
  typename Operation::result_type
  operator()(const typename Operation::second_argument_type& x) const {
    return op(value, x); 
  }
};
```

## 7.6 证同、选择、投射

> 证同元素（identity element)

**运算op的证同元素**：数值A若与该元素做op运算，会得到自己。例如：加法的证同元素位0（因为任何元素加上0仍为自己），乘法的证同元素为1（因为任何元素乘以1仍为自己）

该函数并非STL标准规格的一员

```c++
template <class T> inline T identity_element(plus<T>) { return T(0); }

template <class T> inline T identity_element(multiplies<T>) { return T(1); }
```

> 证同函数（identity function)

**证同函数**：任何数值通过该函数后，不会有任何改变

**使用例子**：`<stl_set.h>`中，用来指定RB-tree所需的KeyOfValue op，因为set元素的键值即实值

```c++
template <class T>
struct identity : public unary_function<T, T> {
  const T& operator()(const T& x) const { return x; }
};

```

> 选择函数（selection function)

选择函数有两个：

- **select1st()**：接受一个pair，传回其第一元素
  - 使用例子：运用于`<stl_map.h>`，用来指定RB-tree所需的KeyOfValue op
- **select2nd()**：接受一个pair，传回其第二个元素

```c++
template <class Pair>
struct select1st : public unary_function<Pair, typename Pair::first_type> {
  const typename Pair::first_type& operator()(const Pair& x) const
  {
    return x.first;
  }
};

template <class Pair>
struct select2nd : public unary_function<Pair, typename Pair::second_type> {
  const typename Pair::second_type& operator()(const Pair& x) const
  {
    return x.second;
  }
};
```

> 投射函数

投射函数有两个

```c++
//投射函数1：传回第一参数，忽略第二参数
template <class Arg1, class Arg2>
struct project1st : public binary_function<Arg1, Arg2, Arg1> {
  Arg1 operator()(const Arg1& x, const Arg2&) const { return x; }
};

//投射函数2：忽略第一参数，传回第二参数
template <class Arg1, class Arg2>
struct project2nd : public binary_function<Arg1, Arg2, Arg2> {
  Arg2 operator()(const Arg1&, const Arg2& y) const { return y; }
};
```

# 第8章 配接器

对adapter样式的定义：将一个class的接口转换为另一个class的接口，使其因接口不兼容而不能合作的classes,可以一起运作 

## 8.1 配接器的分类

- 函数配接器：改变仿函数接口
- 容器配接器：改变容器接口
  - queue和stack
- 迭代器配接器：改变迭代器接口

### 8.1.2 迭代器配接器

由`<iterator>`获取，SGI STL将它们实际定义与`<stl_iterator.h>`

迭代器配接器很少以迭代器为直接参数，其修饰功能只是一种观念上的改变（赋值操作变为插入操作、前进变后退、绑定到特殊装置等）

**分类**：

- insert iterator
- reverse iterator
- iostream iterator

> Insert Iterator

**Insert Iterator**：将一般迭代器的赋值操作转变为插入操作

**包括**：

- back_insert_iterator：尾端插入操作
- front_insert_iterator：头端插入操作
- insert_iterator：任意位置执行插入操作

由于三个iterator adapter接口不直观，STL提供了三个相应函数

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-1.png)

> Reverse Iterator

**Reverse Iterator**：将一般迭代器的行进方向逆转，使原本应该前进的operator++变成后退操作，operator--变为前进操作

> IOStream Iterator

**IOStream Iterator**：将迭代器绑定到某个iostream对象上

**分类**：

- istream_iterator：绑定到istream对象（例如std::cin）身上的，拥有输入功能
- ostream_iterator：绑定到ostream对象（例如std::cout）身上的，拥有输出功能

> 例子

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-%E8%BF%AD%E4%BB%A3%E5%99%A8%E9%85%8D%E6%8E%A5%E5%99%A8%E4%BE%8B%E5%AD%901.png)

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-%E8%BF%AD%E4%BB%A3%E5%99%A8%E9%85%8D%E6%8E%A5%E5%99%A8%E4%BE%8B%E5%AD%902.png)

### 8.1.3 函数配接器

函数配接器操作包括bind、negate、compose、以及对一般函数或成员函数的修饰（使其成为一个仿函数）

函数配接器的接口由`<functional>`获得，SGI STL将其实际定义于`<stl_function.h>`

所有期望获得配接能力的组件，本身都必须是**可配接的**：

- 1.一元仿函数必须继承自unary_function
- 2.二元仿函数必须继承自binary_function
- 3.成员函数必须以mem_fun处理过
- 4.一般函数必须以ptr_fun处理过

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-2.png)

## 8.3 迭代器配接器

### 8.3.1 insert iterators

**主要观念**：每个insert iterators内部维护一个容器（由用户传入）；当客户对insert iterators做赋值操作时，就在insert iterators中被转为对该容器的插入操作，即insert iterators的operator=操作中调用底层容器的push_front()、push_back()、insert()。而加加、减减、解引用和间接引用操作的功能都被关闭。因此类型被定义为**output_iterator_tag**

> **back_insert_iterator**

```c++
template <class Container>
class back_insert_iterator {
protected:
  Container* container;
public:
  typedef output_iterator_tag iterator_category;
  typedef void                value_type;
  typedef void                difference_type;
  typedef void                pointer;
  typedef void                reference;

  explicit back_insert_iterator(Container& x) : container(&x) {}
  back_insert_iterator<Container>&
  operator=(const typename Container::value_type& value) { 
    container->push_back(value);
    return *this;
  }
  back_insert_iterator<Container>& operator*() { return *this; }
  back_insert_iterator<Container>& operator++() { return *this; }
  back_insert_iterator<Container>& operator++(int) { return *this; }
};

//辅助函数
template <class Container>
inline back_insert_iterator<Container> back_inserter(Container& x) {
  return back_insert_iterator<Container>(x);
}
```

> **front_insert_iterator**

```c++
template <class Container>
class front_insert_iterator {
protected:
  Container* container;
public:
  typedef output_iterator_tag iterator_category;
  typedef void                value_type;
  typedef void                difference_type;
  typedef void                pointer;
  typedef void                reference;

  explicit front_insert_iterator(Container& x) : container(&x) {}
  front_insert_iterator<Container>&
  operator=(const typename Container::value_type& value) { 
    container->push_front(value);
    return *this;
  }
  front_insert_iterator<Container>& operator*() { return *this; }
  front_insert_iterator<Container>& operator++() { return *this; }
  front_insert_iterator<Container>& operator++(int) { return *this; }
};

//辅助函数
template <class Container>
inline front_insert_iterator<Container> front_inserter(Container& x) {
  return front_insert_iterator<Container>(x);
}
```

> **insert_iterator**

```c++
template <class Container>
class insert_iterator {
protected:
  Container* container;
  typename Container::iterator iter;
public:
  typedef output_iterator_tag iterator_category;
  typedef void                value_type;
  typedef void                difference_type;
  typedef void                pointer;
  typedef void                reference;

  insert_iterator(Container& x, typename Container::iterator i) 
    : container(&x), iter(i) {}
  insert_iterator<Container>&
  operator=(const typename Container::value_type& value) { 
    iter = container->insert(iter, value);
    ++iter;
    return *this;
  }
  insert_iterator<Container>& operator*() { return *this; }
  insert_iterator<Container>& operator++() { return *this; }
  insert_iterator<Container>& operator++(int) { return *this; }
};

//辅助函数
template <class Container, class Iterator>
inline insert_iterator<Container> inserter(Container& x, Iterator i) {
  typedef typename Container::iterator iter;
  return insert_iterator<Container>(x, iter(i));
}
```

### 8.3.2 reverse iterators

实现了rbegin()和rend()函数的容器，实际上这两个函数都是使用的reverse iterators

```c++
template <class T, class Alloc = alloc>
class vector {
  reverse_iterator rbegin() { return reverse_iterator(end()); }
  const_reverse_iterator rbegin() const { 
    return const_reverse_iterator(end()); 
  }
};
```

"正向迭代器“与”与其相应的逆向迭代器“取出的元素不同，主要是为了配合迭代器区间的”前闭后开“习惯

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-3.png)

```c++
template <class Iterator>
class reverse_iterator 
{
protected:
  Iterator current;  //记录对应的正向迭代器
public:
  typedef typename iterator_traits<Iterator>::iterator_category
          iterator_category;
  typedef typename iterator_traits<Iterator>::value_type
          value_type;
  typedef typename iterator_traits<Iterator>::difference_type
          difference_type;
  typedef typename iterator_traits<Iterator>::pointer
          pointer;
  typedef typename iterator_traits<Iterator>::reference
          reference;

  typedef Iterator iterator_type;  //代表正向迭代器
  typedef reverse_iterator<Iterator> self;  //代表逆向迭代器

public:
  reverse_iterator() {}
  //以下两个构造函数将reverse iterator与某个迭代器x系起来
  explicit reverse_iterator(iterator_type x) : current(x) {}
  reverse_iterator(const self& x) : current(x.current) {}
  
  //取出对应的正向迭代器
  iterator_type base() const { return current; }
  //对逆向迭代器取址，就是将“对应的正向迭代器”后退一格而取值
  reference operator*() const {
    Iterator tmp = current;
    return *--tmp;
  }

  pointer operator->() const { return &(operator*()); }

  self& operator++() {
    --current;
    return *this;
  }
  self operator++(int) {
    self tmp = *this;
    --current;
    return tmp;
  }
  self& operator--() {
    ++current;
    return *this;
  }
  self operator--(int) {
    self tmp = *this;
    ++current;
    return tmp;
  }

  self operator+(difference_type n) const {
    return self(current - n);
  }
  self& operator+=(difference_type n) {
    current -= n;
    return *this;
  }
  self operator-(difference_type n) const {
    return self(current + n);
  }
  self& operator-=(difference_type n) {
    current += n;
    return *this;
  }
  //第一个*和唯一一个+都调用本类的operator*和operator+
  //第二个*根据处理的类型而定
  reference operator[](difference_type n) const { return *(*this + n); }  
}; 
```

### 8.3.3 stream iterators

> **istream iterators**

绑定一个istream object，其实就是在istream iterators内部维护一个istream member，客户对这个迭代器所做的operator++操作，会被导引调用迭代器内部内含的那个istream member的输入操作（operator >>）

该迭代器是Input Iterators，不具备operator--

**注意**：只要客户定义一个istream iterator并**绑定到某个istream object**，程序便立刻停止在`istream_iterator<T>::read()`函数，等待输入

```c++
template <class T, class Distance = ptrdiff_t> 
class istream_iterator {
  friend bool
  //__STL_NULL_TMPL_ARGS被定义为<>
  operator== __STL_NULL_TMPL_ARGS (const istream_iterator<T, Distance>& x,
                                   const istream_iterator<T, Distance>& y);
protected:
  //istream成员
  istream* stream;
  T value;
  bool end_marker;
  void read() {
    //stream是否读到eof,true表示没有，false表示读到eof
    end_marker = (*stream) ? true : false;
    if (end_marker) *stream >> value;
    end_marker = (*stream) ? true : false;
  }
public:
  typedef input_iterator_tag iterator_category;
  typedef T                  value_type;
  typedef Distance           difference_type;
  typedef const T*           pointer;
  typedef const T&           reference;

  //该构造函数可用于生产尾迭代器，istream_iterator<int> eos;
  istream_iterator() : stream(&cin), end_marker(false) {}
  //引发read(),程序停留在此
  //istream_iterator<int> initer(cin);
  istream_iterator(istream& s) : stream(&s) { read(); }
  reference operator*() const { return value; }
  pointer operator->() const { return &(operator*()); }
  
  //迭代器前进一个位置，代表要读一次
  istream_iterator<T, Distance>& operator++() { 
    read(); 
    return *this;
  }
  istream_iterator<T, Distance> operator++(int)  {
    istream_iterator<T, Distance> tmp = *this;
    read();
    return tmp;
  }
};
```

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-4.png)

> **ostream iterators**

绑定一个ostream object，其实就是在ostream iterators内部维护一个ostream member，客户对这个迭代器所做的operator=操作，会被导引调用迭代器内部内含的那个istream member的输出操作（operator <<）

该迭代器是Output Iterators

```c++
template <class T>
class ostream_iterator {
protected:
  ostream* stream;  //ostream成员
  const char* string;  //每次输出后的间隔符
public:
  typedef output_iterator_tag iterator_category;
  typedef void                value_type;
  typedef void                difference_type;
  typedef void                pointer;
  typedef void                reference;

  ostream_iterator(ostream& s) : stream(&s), string(0) {}
  //用法：ostream_iterator<int> outiter(cout, ' '); 
  //输出至cout，每次间隔一空格
  ostream_iterator(ostream& s, const char* c) : stream(&s), string(c)  {}
  //对迭代器做赋值操作，就代表要输出一次
  ostream_iterator<T>& operator=(const T& value) { 
    *stream << value;  //先输出被赋予的数值
    if (string) *stream << string;  //如果间隔符号不为空，输出间隔符号
    return *this;
  }
  //注意以下三个操作
  ostream_iterator<T>& operator*() { return *this; }
  ostream_iterator<T>& operator++() { return *this; } 
  ostream_iterator<T>& operator++(int) { return *this; } 
};
```

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-5.png)

## 8.4 function adapters

每个function adapters内藏了一个数据成员，其类型等同于它所要配接的对象（一个“可配接的仿函数”）

**例子**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-7.png)

**function adpaters汇总**：

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-6.png)

### 8.4.1 对返回值进行逻辑否定 not1 not2

- **not1**：对一个参数的仿函数的结果进行否定
- **not2**：对两个参数的仿函数的结果进行否定

> **not1**

```c++
//unary_negate配接器用来表示某个Adapter Predicate的逻辑负值
template <class Predicate>
class unary_negate
  : public unary_function<typename Predicate::argument_type, bool> {
protected:
  Predicate pred; //内部成员
public:
  explicit unary_negate(const Predicate& x) : pred(x) {}
  bool operator()(const typename Predicate::argument_type& x) const {
    return !pred(x);
  }
};

//辅助函数，利用模板函数的自动推断，使得我们得以方便使用unary_negate<Pred>
template <class Predicate>
inline unary_negate<Predicate> not1(const Predicate& pred) {
  return unary_negate<Predicate>(pred);
}
```

> **not2**

```c++
//binary_negate配接器用来表示某个Adapter Predicate的逻辑负值
template <class Predicate> 
class binary_negate 
  : public binary_function<typename Predicate::first_argument_type,
                           typename Predicate::second_argument_type,
                           bool> {
protected:
  Predicate pred; //内部成员
public:
  explicit binary_negate(const Predicate& x) : pred(x) {}
  bool operator()(const typename Predicate::first_argument_type& x, 
                  const typename Predicate::second_argument_type& y) const {
    return !pred(x, y); 
  }
};

//辅助函数，利用模板函数的自动推断，使得我们得以方便使用binary_negate<Pred>
template <class Predicate>
inline binary_negate<Predicate> not2(const Predicate& pred) {
  return binary_negate<Predicate>(pred);
}
```

### 8.4.2 对参数进行绑定 bind1st bind2nd

- **bind1st**：`bind1st(less<int>,12)`将12绑定为`less<int>`的第1个参数，即变成`less<int>(12,x)`，使用`bind1st(less<int>,12)`时只需传入第2个参数即可
- **bind2nd**：`bind2nd(less<int>,12)`将12绑定为`less<int>`的第2个参数，即变成`less<int>(x,12)`，使用`bind2nd(less<int>,12)`时只需传入第1个参数即可

> **bind1st** 

```c++
template <class Operation> 
class binder1st
  : public unary_function<typename Operation::second_argument_type,
                          typename Operation::result_type> {
protected:
  Operation op;
  typename Operation::first_argument_type value;
public:
  binder1st(const Operation& x,
            const typename Operation::first_argument_type& y)
      : op(x), value(y) {}
  typename Operation::result_type
  operator()(const typename Operation::second_argument_type& x) const {
    return op(value, x);  //将value绑定为参数1
  }
};

//辅助函数
template <class Operation, class T>
inline binder1st<Operation> bind1st(const Operation& op, const T& x) {
  typedef typename Operation::first_argument_type arg1_type;
  return binder1st<Operation>(op, arg1_type(x));
}
```

> **bind2nd**

```c++
template <class Operation> 
class binder2nd
  : public unary_function<typename Operation::first_argument_type,
                          typename Operation::result_type> {
protected:
  Operation op;
  typename Operation::second_argument_type value;
public:
  binder2nd(const Operation& x,
            const typename Operation::second_argument_type& y) 
      : op(x), value(y) {}
  typename Operation::result_type
  operator()(const typename Operation::first_argument_type& x) const {
    return op(x, value); //将value绑定为参数2
  }
};

//辅助函数
template <class Operation, class T>
inline binder2nd<Operation> bind2nd(const Operation& op, const T& x) {
  typedef typename Operation::second_argument_type arg2_type;
  return binder2nd<Operation>(op, arg2_type(x));
}
```

### 8.4.3 用于函数合成 compose1 compose2

本节的两个配接器并未纳入STL标准，是SGI STL的私产品

- **compose1**：已知两个可配接的单参数仿函数f()，g()，compose1用来产生一个h(),使`h(x) = f(g(x))`
- **compose2**：已知一个可匹配的双参数仿函数f和两个可配接的单参数仿函数g1，g2，compose2用来产生一个h(),使`h(x) = f(g1(x),g2(x))`

> **compose1**

```c++
template <class Operation1, class Operation2>
class unary_compose : public unary_function<typename Operation2::argument_type,
                                            typename Operation1::result_type> {
protected:
  Operation1 op1;  //内部成员
  Operation2 op2;  //内部成员
public:
  unary_compose(const Operation1& x, const Operation2& y) : op1(x), op2(y) {}
  typename Operation1::result_type
  //注意这里是op2的参数类型
  operator()(const typename Operation2::argument_type& x) const {
    return op1(op2(x)); 
  }
};

//辅助函数
template <class Operation1, class Operation2>
inline unary_compose<Operation1, Operation2> compose1(const Operation1& op1, 
                                                      const Operation2& op2) {
  return unary_compose<Operation1, Operation2>(op1, op2);
}
```

> **compose2**

```c++
template <class Operation1, class Operation2, class Operation3>
class binary_compose
  : public unary_function<typename Operation2::argument_type,
                          typename Operation1::result_type> {
protected:
  Operation1 op1;
  Operation2 op2;
  Operation3 op3;
public:
  binary_compose(const Operation1& x, const Operation2& y, 
                 const Operation3& z) : op1(x), op2(y), op3(z) { }
  typename Operation1::result_type
  //从这里可知op2和op3的参数类型必须相同
  operator()(const typename Operation2::argument_type& x) const {
    return op1(op2(x), op3(x));
  }
};

//辅助函数
template <class Operation1, class Operation2, class Operation3>
inline binary_compose<Operation1, Operation2, Operation3> 
compose2(const Operation1& op1, const Operation2& op2, const Operation3& op3) {
  return binary_compose<Operation1, Operation2, Operation3>(op1, op2, op3);
}
```

### 8.4.4 用于函数指针 ptr_fun

ptr_fun配接器使我们能够将一般函数当做仿函数使用

- `ptr_fun(Result (*x)(Arg))`将一元函数转换成一元仿函数
- `ptr_fun(Result (*x)(Arg1, Arg2))`将二元函数转换成二元仿函数

> **ptr_fun(Result (*x)(Arg))**

```c++
//以下配接器其实就是把一个一元函数指针包起来
//当仿函数被使用时，就调用该函数指针
template <class Arg, class Result>
class pointer_to_unary_function : public unary_function<Arg, Result> {
protected:
  Result (*ptr)(Arg);  //函数指针
public:
  pointer_to_unary_function() {}
  explicit pointer_to_unary_function(Result (*x)(Arg)) : ptr(x) {}
  //通过函数指针执行函数
  Result operator()(Arg x) const { return ptr(x); }
};

//辅助函数
template <class Arg, class Result>
inline pointer_to_unary_function<Arg, Result> ptr_fun(Result (*x)(Arg)) {
  //返回一个仿函数对象
  return pointer_to_unary_function<Arg, Result>(x);
}
```

> **ptr_fun(Result (*x)(Arg1, Arg2))**

```c++
//以下配接器其实就是把一个二元函数指针包起来
//当仿函数被使用时，就调用该函数指针
template <class Arg1, class Arg2, class Result>
class pointer_to_binary_function : public binary_function<Arg1, Arg2, Result> {
protected:
    Result (*ptr)(Arg1, Arg2);  //函数指针
public:
    pointer_to_binary_function() {}
    explicit pointer_to_binary_function(Result (*x)(Arg1, Arg2)) : ptr(x) {}
    //通过函数指针执行函数
    Result operator()(Arg1 x, Arg2 y) const { return ptr(x, y); }
};

template <class Arg1, class Arg2, class Result>
inline pointer_to_binary_function<Arg1, Arg2, Result> 
ptr_fun(Result (*x)(Arg1, Arg2)) {
  //返回一个仿函数对象
  return pointer_to_binary_function<Arg1, Arg2, Result>(x);
}
```

### 8.4.5 用于成员函数指针 mem_fun mem_fun_ref

这两种配接器使我们能够将成员函数当做仿函数使用

当容器元素的类型是X&或X*时，可以实现多态

![](../../pics/language/STL%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90/img-8-8.png)

该族群一个有8个仿函数

- 1.无任何参数 vs 有一个参数
- 2.通过指针调用 vs 通过引用调用
- 3.const成员函数 vs 非const成员函数

```c++
//1. 无任何参数 通过指针调用 非const成员函数
template <class S, class T>
class mem_fun_t : public unary_function<T*, S> {
public:
  explicit mem_fun_t(S (T::*pf)()) : f(pf) {}
  //通过指针调用
  S operator()(T* p) const { return (p->*f)(); }
private:
  S (T::*f)(); //无任何参数 非const成员函数
};

//2. 无任何参数 通过指针调用 const成员函数
template <class S, class T>
class const_mem_fun_t : public unary_function<const T*, S> {
public:
  explicit const_mem_fun_t(S (T::*pf)() const) : f(pf) {}
  //通过指针调用
  S operator()(const T* p) const { return (p->*f)(); }
private:
  S (T::*f)() const; //无任何参数 const成员函数
};

//3. 无任何参数 通过引用调用 非const成员函数
template <class S, class T>
class mem_fun_ref_t : public unary_function<T, S> {
public:
  explicit mem_fun_ref_t(S (T::*pf)()) : f(pf) {}
  //通过引用调用
  S operator()(T& r) const { return (r.*f)(); }
private:
  S (T::*f)(); //无任何参数 非const成员函数
};

//4. 无任何参数 通过引用调用 const成员函数
template <class S, class T>
class const_mem_fun_ref_t : public unary_function<T, S> {
public:
  explicit const_mem_fun_ref_t(S (T::*pf)() const) : f(pf) {}
  //通过引用调用
  S operator()(const T& r) const { return (r.*f)(); }
private:
  S (T::*f)() const; //无任何参数 const成员函数
};

//5. 有一个参数 通过指针调用 非const成员函数
template <class S, class T, class A>
class mem_fun1_t : public binary_function<T*, A, S> {
public:
  explicit mem_fun1_t(S (T::*pf)(A)) : f(pf) {}
  //通过指针调用
  S operator()(T* p, A x) const { return (p->*f)(x); }
private:
  S (T::*f)(A); //有一个参数 非const成员函数
};

//6. 有一个参数 通过指针调用 const成员函数
template <class S, class T, class A>
class const_mem_fun1_t : public binary_function<const T*, A, S> {
public:
  explicit const_mem_fun1_t(S (T::*pf)(A) const) : f(pf) {}
  //通过指针调用
  S operator()(const T* p, A x) const { return (p->*f)(x); }
private:
  S (T::*f)(A) const; //有一个参数 const成员函数
};

//7. 有一个参数 通过引用调用 非const成员函数
template <class S, class T, class A>
class mem_fun1_ref_t : public binary_function<T, A, S> {
public:
  explicit mem_fun1_ref_t(S (T::*pf)(A)) : f(pf) {}
  //通过引用调用
  S operator()(T& r, A x) const { return (r.*f)(x); }
private:
  S (T::*f)(A); //有一个参数 非const成员函数
};

//8. 有一个参数 通过引用调用 const成员函数
template <class S, class T, class A>
class const_mem_fun1_ref_t : public binary_function<T, A, S> {
public:
  explicit const_mem_fun1_ref_t(S (T::*pf)(A) const) : f(pf) {}
  //通过引用调用
  S operator()(const T& r, A x) const { return (r.*f)(x); }
private:
  S (T::*f)(A) const; //有一个参数 const成员函数
};
```

所有的复杂都只存在于仿函数内部，我们可以忽略它们，直接使用更上层的辅助函数mem_fun和mem_fun_ref

辅助函数：

```c++
template <class S, class T>
inline mem_fun_t<S,T> mem_fun(S (T::*f)()) { 
  return mem_fun_t<S,T>(f);
}

template <class S, class T>
inline const_mem_fun_t<S,T> mem_fun(S (T::*f)() const) {
  return const_mem_fun_t<S,T>(f);
}

template <class S, class T>
inline mem_fun_ref_t<S,T> mem_fun_ref(S (T::*f)()) { 
  return mem_fun_ref_t<S,T>(f);
}

template <class S, class T>
inline const_mem_fun_ref_t<S,T> mem_fun_ref(S (T::*f)() const) {
  return const_mem_fun_ref_t<S,T>(f);
}

template <class S, class T, class A>
inline mem_fun1_t<S,T,A> mem_fun1(S (T::*f)(A)) { 
  return mem_fun1_t<S,T,A>(f);
}

template <class S, class T, class A>
inline const_mem_fun1_t<S,T,A> mem_fun1(S (T::*f)(A) const) {
  return const_mem_fun1_t<S,T,A>(f);
}

template <class S, class T, class A>
inline mem_fun1_ref_t<S,T,A> mem_fun1_ref(S (T::*f)(A)) { 
  return mem_fun1_ref_t<S,T,A>(f);
}

template <class S, class T, class A>
inline const_mem_fun1_ref_t<S,T,A> mem_fun1_ref(S (T::*f)(A) const) {
  return const_mem_fun1_ref_t<S,T,A>(f);
}
```

















