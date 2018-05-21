## deque-operator--()

```c++
  self& operator--() {
    if (cur == first) {    //如果已经到达缓冲区的头端
      set_node(node - 1);  //就切换至上一个节点（缓冲区）
      cur = last;          // 的最后一个元素的下一位置
    }
    --cur;                 //切换至前一个元素，即last之前的那个元素
    return *this;
  }
```

