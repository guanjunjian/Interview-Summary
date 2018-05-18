#### uninitialized_copy()

```c++
template <class InputIterator, class ForwardIterator>
inline ForwardIterator
  uninitialized_copy(InputIterator first, InputIterator last,
                     ForwardIterator result) {
  //利用value_type()取出result的value type
  return __uninitialized_copy(first, last, result, value_type(result));
}

template <class InputIterator, class ForwardIterator, class T>
inline ForwardIterator
__uninitialized_copy(InputIterator first, InputIterator last,
                     ForwardIterator result, T*) {
  //首先萃取出迭代器result的value type，然后判断该类型是否为POD类型
  typedef typename __type_traits<T>::is_POD_type is_POD;
  return __uninitialized_copy_aux(first, last, result, is_POD());
}

// 如果copy construction等同于assignment，而且destructor是trivial，以下就有效
// 如果是POD类型，执行流程就会转到该函数
template <class InputIterator, class ForwardIterator>
inline ForwardIterator 
__uninitialized_copy_aux(InputIterator first, InputIterator last,
                         ForwardIterator result,
                         __true_type) {
  return copy(first, last, result);//交给高阶函数执行，STL的copy()
}

//如果不是POD类型，执行流程会转进该函数
template <class InputIterator, class ForwardIterator>
ForwardIterator 
__uninitialized_copy_aux(InputIterator first, InputIterator last,
                         ForwardIterator result,
                         __false_type) {
  ForwardIterator cur = result;
  //异常机制保证要么所有对象都构造成功，要么一个都没构造
  __STL_TRY { //d efine __STL_TRY try
    for ( ; first != last; ++first, ++cur)
        //必须一个一个元素地构造，无法批量处理
        construct(&*cur, *first); 
    return cur;
  }
  //define __STL_UNWIND(action) catch(...) { action; throw; }
  __STL_UNWIND(destroy(result, cur)); //如果构造出现异常，需要析构
}

//针对first为char*的特化版本
inline char* uninitialized_copy(const char* first, const char* last,
                                char* result) {
    //采用最优效率的memmove（直接移动内存内容）来执行复制行为
    memmove(result, first, last - first);
    return result + (last - first);
}

//针对first为wchar_t*的特化版本
inline wchar_t* uninitialized_copy(const wchar_t* first, const wchar_t* last,
                                   wchar_t* result) {
   //采用最优效率的memmove（直接移动内存内容）来执行复制行为
  memmove(result, first, sizeof(wchar_t) * (last - first));
  return result + (last - first);
}
```

