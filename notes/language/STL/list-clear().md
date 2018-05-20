## list-clear()

```c++
//清除所有节点（整个链表）
template <class T, class Alloc> 
void list<T, Alloc>::clear()
{
  //node为空节点，它的下一个节点是begin()
  link_type cur = (link_type) node->next;
  //循环遍历每个节点
  while (cur != node) {
    link_type tmp = cur;
    cur = (link_type) cur->next;
    //销毁（析构并释放）一个节点
    destroy_node(tmp);
  }
  //恢复node原始状态
  node->next = node;
  node->prev = node;
}
```

