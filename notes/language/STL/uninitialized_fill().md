## uninitialized_fill()

```c++
template <class ForwardIterator, class T>
inline void uninitialized_fill(ForwardIterator first, ForwardIterator last, 
                               const T& x) {
  //利用value_type()取出first的value type
  __uninitialized_fill(first, last, x, value_type(first));
}

template <class ForwardIterator, class T, class T1>
inline void __uninitialized_fill(ForwardIterator first, ForwardIterator last, 
                                 const T& x, T1*) {
  //首先萃取出迭代器first的value type，然后判断该类型是否为POD类型
  typedef typename __type_traits<T1>::is_POD_type is_POD;
  __uninitialized_fill_aux(first, last, x, is_POD());                 
}

// 如果是POD类型，执行流程就会转到以下函数
template <class ForwardIterator, class T>
inline void
__uninitialized_fill_aux(ForwardIterator first, ForwardIterator last, 
                         const T& x, __true_type)
{
  fill(first, last, x);//交给高阶函数执行，调用STL算法fill()
}

//如果不是POD类型，执行流程会转进以下函数
template <class ForwardIterator, class T>
void
__uninitialized_fill_aux(ForwardIterator first, ForwardIterator last, 
                         const T& x, __false_type)
{
  ForwardIterator cur = first;
  //异常机制保证要么所有对象都构造成功，要么一个都没构造
  __STL_TRY { //d efine __STL_TRY try
    for ( ; cur != last; ++cur)
      //必须一个一个元素构造，无法批量处理  
      construct(&*cur, x); 
  }
  //define __STL_UNWIND(action) catch(...) { action; throw; }
  __STL_UNWIND(destroy(first, cur)); //如果构造出现异常，需要析构
}
```

