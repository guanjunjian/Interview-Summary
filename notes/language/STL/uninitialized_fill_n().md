## uninitialized_fill_n()

```c++
template <class ForwardIterator, class Size, class T>
inline ForwardIterator uninitialized_fill_n(ForwardIterator first, Size n,
                                            const T& x) {
  //利用value_type()取出first的value type
  return __uninitialized_fill_n(first, n, x, value_type(first));
}

//首先萃取出迭代器first的value type，然后判断该类型是否为POD类型
template <class ForwardIterator, class Size, class T, class T1>
inline ForwardIterator __uninitialized_fill_n(ForwardIterator first, Size n,
                                              const T& x, T1*) {
  //__type_traits<>技巧法，详见3.7节
  typedef typename __type_traits<T1>::is_POD_type is_POD;
  return __uninitialized_fill_n_aux(first, n, x, is_POD());             
}

//如果copy construction等同于assignment，而且destructor是trivial，以下就有效
//如果是POD类型，执行流程就会转到该函数
//这是由函数模板的参数推导机制而得
template <class ForwardIterator, class Size, class T>
inline ForwardIterator
__uninitialized_fill_n_aux(ForwardIterator first, Size n,
                           const T& x, __true_type) {
  return fill_n(first, n, x);//交给高阶函数执行
}

//如果不是POD类型，执行流程会转进该函数
template <class ForwardIterator, class Size, class T>
ForwardIterator
__uninitialized_fill_n_aux(ForwardIterator first, Size n,
                           const T& x, __false_type) {
  ForwardIterator cur = first;
  //异常机制保证要么所有对象都构造成功，要么一个都没构造
  __STL_TRY { //d efine __STL_TRY try
    for ( ; n > 0; --n, ++cur)
      construct(&*cur, x);
    return cur;
  }
  //define __STL_UNWIND(action) catch(...) { action; throw; }
  __STL_UNWIND(destroy(first, cur)); //如果构造出现异常，需要析构
}
```

