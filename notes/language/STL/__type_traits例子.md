## __type_traits例子---uninitialized_fill_n()

```c++
//uninitialized_fill_n()
template <class ForwardIterator, class Size, class T>
inline ForwardIterator uninitialized_fill_n(ForwardIterator first, Size n,
                                            const T& x) {
  //通过value_type()萃取出迭代器first的value type，
  //再利用__type_traits判断该类型是否POD类型
  return __uninitialized_fill_n(first, n, x, value_type(first));
}

//value_type()
template <class Iterator>
inline typename iterator_traits<Iterator>::value_type*
value_type(const Iterator&) {
  return static_cast<typename iterator_traits<Iterator>::value_type*>(0);
}

//__uninitialized_fill_n()
template <class ForwardIterator, class Size, class T, class T1>
inline ForwardIterator __uninitialized_fill_n(ForwardIterator first, Size n,
                                              const T& x, T1*) {
  //根据__type_traits判断该类型是否POD类型
  typedef typename __type_traits<T1>::is_POD_type is_POD;
  //生成一个is_POD类型的临时对象
  return __uninitialized_fill_n_aux(first, n, x, is_POD());               
}

//__uninitialized_copy_aux(...,__true_type)
template <class InputIterator, class ForwardIterator>
inline ForwardIterator 
__uninitialized_copy_aux(InputIterator first, InputIterator last, ForwardIterator result, __true_type) {
}

//__uninitialized_copy_aux(...,__false_type)
template <class InputIterator, class ForwardIterator>
inline ForwardIterator 
__uninitialized_copy_aux(InputIterator first, InputIterator last, ForwardIterator result, __false_type) {
}

```

