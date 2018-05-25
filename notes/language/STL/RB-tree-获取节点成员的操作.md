## RB-tree-获取节点成员的操作

- left()
- right()
- parent()
- value()
- key()
- color()

```c++
class rb_tree {
    //...
  //参数类型为link_type，即rb_tree_node*
  static link_type& left(link_type x) { return (link_type&)(x->left); }
  static link_type& right(link_type x) { return (link_type&)(x->right); }
  static link_type& parent(link_type x) { return (link_type&)(x->parent); }
  static reference value(link_type x) { return x->value_field; }
  static const Key& key(link_type x) { return KeyOfValue()(value(x)); }
  static color_type& color(link_type x) { return (color_type&)(x->color); }

  //参数类型为base_ptr，即__rb_tree_node_base*
  static link_type& left(base_ptr x) { return (link_type&)(x->left); }
  static link_type& right(base_ptr x) { return (link_type&)(x->right); }
  static link_type& parent(base_ptr x) { return (link_type&)(x->parent); }
  static reference value(base_ptr x) { return ((link_type)x)->value_field; }
  static const Key& key(base_ptr x) { return KeyOfValue()(value(link_type(x)));} 
  static color_type& color(base_ptr x) { return (color_type&)(link_type(x)->color); }    
    //...
};
```

