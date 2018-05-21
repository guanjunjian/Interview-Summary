## deque-operator+=(n)

```c++
  //实现随机存取，迭代器可直接跳跃n个距离
  self& operator+=(difference_type n) {
    difference_type offset = n + (cur - first);
    //如果目标位置在同一缓冲区内
    if (offset >= 0 && offset < difference_type(buffer_size()))
      cur += n;
    else { //如果目标位置不在同一缓冲区内
      difference_type node_offset =
        offset > 0 ? offset / difference_type(buffer_size())
                   : -difference_type((-offset - 1) / buffer_size()) - 1;
      set_node(node + node_offset);
      cur = first + (offset - node_offset * difference_type(buffer_size()));
    }
    return *this;
  }
```



