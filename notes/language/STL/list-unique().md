## list-unique()

```c++
//移除数值相同的连续节点，注意，只有“连续而相同的元素”，才会被移除剩一个
template <class T, class Alloc>
void list<T, Alloc>::unique() {
  iterator first = begin();
  iterator last = end();
  //如果是空链表，则什么都不做
  if (first == last) return;
  iterator next = first;
  //遍历每一个节点
  while (++next != last) {
    //如果在此区段中有相同的元素，移除它
    if (*first == *next)
      erase(next);
    else
      first = next;
    next = first;
  }
}
```

