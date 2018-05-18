## 以advance()为例

下面定义5个类，代表5个迭代器类型，用于函数重载。函数重载时必须加上一个类型已经确定的函数参数

```c++
struct input_iterator_tag {};
struct output_iterator_tag {};
struct forward_iterator_tag : public input_iterator_tag {};
struct bidirectional_iterator_tag : public forward_iterator_tag {};
struct random_access_iterator_tag : public bidirectional_iterator_tag {};
```

> __advance()函数的重载

```c++
//InputIterator
template <class InputIterator, class Distance>
inline void __advance(InputIterator& i, Distance n, input_iterator_tag) {
  //单向，逐一前进
  while (n--) ++i;
}

//ForwardIterator
template <class InputIterator, class Distance>
inline void __advance(ForwardIterator& i, Distance n, forward_iterator_tag) {
  //单纯地进行传递调用，该函数不需要实现，稍后“消除“单纯传递调用的函数””讨论如何免除
  __advance(i, n, input_iterator_tag());
}

//BidirectionalIterator
template <class BidirectionalIterator, class Distance>
inline void __advance(BidirectionalIterator& i, Distance n, 
                      bidirectional_iterator_tag) {
  //双向，逐一前进  
  if (n >= 0)
    while (n--) ++i;
  else
    while (n++) --i;
}

//RandomAccessIterator
template <class RandomAccessIterator, class Distance>
inline void __advance(RandomAccessIterator& i, Distance n, 
                      random_access_iterator_tag) {
  //双向，跳跃前进  
  i += n;
}
```

**注意**：最后一个参数只声明类型，并未指定参数名，因为它只是用来激活重载机制，函数中根本不使用该函数

> 对外接口advance()

```c++
template <class InputIterator, class Distance>
inline void advance(InputIterator& i, Distance n) {
  //iterator_traits<InputIterator>::iterator_category()将产生一个临时对象，
  //其类型应该隶属于前述4个迭代器类型（I,F,B,R）之一，
  //根据这个类型，编译器决定调用哪一个__advance()重载函数
  __advance(i, n, iterator_traits<InputIterator>::iterator_category());
}
```

**注意**：advance的第一个参数类型为InputIterator，这是STL算法的一个命名规则：以算法所能接受的最低阶迭代器类型，来为其迭代器类型参数命名

> traits中的实现

增加iterator_category类型

```c++
//声明内嵌类型
template <class Iterator>
struct iterator_traits {
  typedef typename Iterator::iterator_category iterator_category;

};

//偏特化：原生指针
template <class T>
struct iterator_traits<T*> {
  //原生指针是一种Random Access Iterator
  typedef random_access_iterator_tag iterator_category;

};

//偏特化：指向常数对象的指针
template <class T>
struct iterator_traits<const T*> {
  //原生的pointer-to-const是一种andom Access Iterator
  typedef random_access_iterator_tag iterator_category;

};
```

> 消除“单纯传递调用的函数”

__advance()函数的重载中，ForwardIterator版本的可以不实现（源码中确实没有实现）

**例子**：

![](../../../pics/language/STL源码剖析/img-3-消除单纯传递调用的函数的例子.png)