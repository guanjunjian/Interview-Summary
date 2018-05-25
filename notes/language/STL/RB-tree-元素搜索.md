## RB-tree-元素搜索

- find()

```c++
template <class Key, class Value, class KeyOfValue, class Compare, class Alloc>
//寻找RB树中是否有键值为k的节点
typename rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::iterator 
rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::find(const Key& k) {
  link_type y = header;        // Last node which is not less than k. 
  link_type x = root();        // Current node. 

  while (x != 0) 
    //key_compare()是节点值大小比较准则，应该会是个function object
    //如果key(x)“小于”k，则向左走
    if (!key_compare(key(x), k))
      y = x, x = left(x);
    else //如果key(x)“大于”k，则向右走
      x = right(x);

  iterator j = iterator(y);   
  return (j == end() || key_compare(k, key(j.node))) ? end() : j;
}
```

