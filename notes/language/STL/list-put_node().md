## list-put_node()

```c++
protected:
  //释放一个节点
  void put_node(link_type p) { list_node_allocator::deallocate(p); }
```

