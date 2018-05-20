## list-create_node()

```c++
  link_type create_node(const T& x) {
    //首先获得一个节点
    link_type p = get_node();
    __STL_TRY {
      //然后根据x构造该节点
      construct(&p->data, x);
    }
    __STL_UNWIND(put_node(p));
    return p;
  }
```

