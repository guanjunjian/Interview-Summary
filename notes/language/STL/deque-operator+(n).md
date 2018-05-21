## deque-operator+(n)

```c++
  self operator+(difference_type n) const {
    self tmp = *this;
    //调用operator+=(n)
    return tmp += n;
  }
```

