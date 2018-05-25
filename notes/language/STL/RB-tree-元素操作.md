## RB-tree-元素操作

- 获取root节点：root()
- 获取最左子节点：leftmost()
- 获取最右子节点：rightmost()
- 获取起始节点：begin()
- 获取末尾节点：end()
- 是否为空：empty()
- 大小：size()
- 可容纳的最大值：max_size()

```c++
  link_type& root() const { return (link_type&) header->parent; }
  link_type& leftmost() const { return (link_type&) header->left; }
  link_type& rightmost() const { return (link_type&) header->right; }
  iterator begin() { return leftmost(); }
  iterator end() { return header; }
  bool empty() const { return node_count == 0; }
  size_type size() const { return node_count; }
  size_type max_size() const { return size_type(-1); }
```

