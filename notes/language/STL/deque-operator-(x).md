## deque-operator-(x)

```c++
  //计算两个迭代器之间的距离
  difference_type operator-(const self& x) const {
    // 缓冲区大小 * 两者之间隔着几个缓冲区 
    return difference_type(buffer_size()) * (node - x.node - 1) +
      (cur - first) + (x.last - x.cur);
```

