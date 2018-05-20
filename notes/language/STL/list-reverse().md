## list-reverse()

```c++
//将*this的内容逆序重置
template <class T, class Alloc>
void list<T, Alloc>::reverse() {
  //如果只有空白节点，或只有一个节点，则什么也不用做
  if (node->next == node || link_type(node->next)->next == node) return;
  iterator first = begin();
  ++first;
  //将一个一个元素移动到begin()之前
  while (first != end()) {
    iterator old = first;
    ++first;
    transfer(begin(), old, first);
  }
}  
```

