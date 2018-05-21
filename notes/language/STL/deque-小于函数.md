## deque-小于函数

```c++
bool operator<(const self& x) const {
    //区分是否在同一个缓冲区内，比较方法不一样
    return (node == x.node) ? (cur < x.cur) : (node < x.node);
  }
```

