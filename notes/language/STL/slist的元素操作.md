## slist的元素操作

```c++
  //1.
  slist() { head.next = 0; }
  //2.
  ~slist() { clear(); }
  //3.
  iterator begin() { return iterator((list_node*)head.next); }
  //4.
  iterator end() { return iterator(0); }
  //5.
  size_type size() const { return __slist_size(head.next); }
  //6.
  bool empty() const { return head.next == 0; }
  //7.
  void swap(slist& L)
  {
    list_node_base* tmp = head.next;
    head.next = L.head.next;
    L.head.next = tmp;
  }
  //8.
  reference front() { return ((list_node*) head.next)->data; }
  //9.
  void push_front(const value_type& x)   {
    __slist_make_link(&head, create_node(x));
  }
  //10.
  void pop_front() {
    list_node* node = (list_node*) head.next;
    head.next = node->next;
    destroy_node(node);
  }
```



