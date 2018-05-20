## list-sort()

```c++
//list不能使用STL算法sort()，必须使用自己的sort()成员函数
//因为STL算法sort()只接受RamdonAccessIterator
//本函数采用快排
template <class T, class Alloc>
void list<T, Alloc>::sort() {
  //如果只有空白节点，或只有一个节点，什么也不做
  if (node->next == node || link_type(node->next)->next == node) return;
  //一些新的lists，作为中介数据存放区
  list<T, Alloc> carry;
  list<T, Alloc> counter[64];
  int fill = 0;
  while (!empty()) {
    carry.splice(carry.begin(), *this, begin());
    int i = 0;
    while(i < fill && !counter[i].empty()) {
      counter[i].merge(carry);
      carry.swap(counter[i++]);
    }
    carry.swap(counter[i]);         
    if (i == fill) ++fill;
  } 

  for (int i = 1; i < fill; ++i) counter[i].merge(counter[i-1]);
  swap(counter[fill-1]);
}
```

