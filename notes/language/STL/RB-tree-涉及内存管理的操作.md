## RB-tree-涉及内存管理的操作

- 分配节点：get_node()
- 释放节点：put_node()
- 创建节点：create_node()
- 拷贝节点：clone_node()
- 销毁节点：destroy_node()

```c++
class rb_tree {
    //...
protected:
  link_type get_node() { return rb_tree_node_allocator::allocate(); }
  void put_node(link_type p) { rb_tree_node_allocator::deallocate(p); }

  link_type create_node(const value_type& x) {
    link_type tmp = get_node();
    __STL_TRY {
      construct(&tmp->value_field, x);
    }
    __STL_UNWIND(put_node(tmp));
    return tmp;
  }

  link_type clone_node(link_type x) {
    link_type tmp = create_node(x->value_field);
    tmp->color = x->color;
    tmp->left = 0;
    tmp->right = 0;
    return tmp;
  }

  void destroy_node(link_type p) {
    destroy(&p->value_field);
    put_node(p);
  }
    //...
};
```

