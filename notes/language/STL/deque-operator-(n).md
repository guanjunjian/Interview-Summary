## deque-operator-(n)

```c++
self operator-(difference_type n) const {
    self tmp = *this;
    //调用operator-=(n)，进一步调用operator+=()
    return tmp -= n;
  }
```

