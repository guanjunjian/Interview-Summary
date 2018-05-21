## deque-operator++()

```c++
  self& operator++() {
    ++cur;                  //切换至下一个元素
    if (cur == last) {      //如果已经到达所在缓冲区的尾端
      set_node(node + 1);   //切换至下一个节点（缓冲区）
      cur = first;          // 的第一个元素
    }
    return *this; 
  }
```

