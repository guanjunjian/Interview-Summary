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

>Insert Iterator

**Insert Iterator**：将一般迭代器的赋值操作转变为插入操作

**包括**：

- back_insert_iterator：尾端插入操作
- front_insert_iterator：头端插入操作
- insert_iterator：任意位置执行插入操作

由于三个iterator adapter接口不直观，STL提供了三个相应函数

![](../../pics/language/STL源码剖析/img-8-1.png)

> Reverse Iterator

**Reverse Iterator**：将一般迭代器的行进方向逆转，使原本应该前进的operator++变成后退操作，operator--变为前进操作

> IOStream Iterator

**IOStream Iterator**：将迭代器绑定到某个iostream对象上

**分类**：

- istream_iterator：绑定到istream对象（例如std::cin）身上的，拥有输入功能
- ostream_iterator：绑定到ostream对象（例如std::cout）身上的，拥有输出功能

> 例子

![](../../pics/language/STL源码剖析/img-8-迭代器配接器例子1.png)

![](../../pics/language/STL源码剖析/img-8-迭代器配接器例子2.png)

### 8.1.3 函数配接器

函数配接器操作包括bind、negate、compose、以及对一般函数或成员函数的修饰（使其成为一个仿函数）

函数配接器的接口由`<functional>`获得，SGI STL将其实际定义于`<stl_function.h>`

所有期望获得配接能力的组件，本身都必须是**可配接的**：

- 1.一元仿函数必须继承自unary_function
- 2.二元仿函数必须继承自binary_function
- 3.成员函数必须以mem_fun处理过
- 4.一般函数必须以ptr_fun处理过

![](../../pics/language/STL源码剖析/img-8-2.png)

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

![](../../pics/language/STL源码剖析/img-8-3.png)

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

![](../../pics/language/STL源码剖析/img-8-4.png)

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

![](../../pics/language/STL源码剖析/img-8-5.png)



