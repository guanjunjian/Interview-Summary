## list-remove()

```c++
//将数值为value的所有元素移除
template <class T, class Alloc>
void list<T, Alloc>::remove(const T& value) {
  iterator first = begin();
  iterator last = end();
  //遍历每一个节点
  while (first != last) {
    iterator next = first;
    ++next;
    //如果值为value，则移除
    if (*first == value) erase(first);
    first = next;
  }
}
```



