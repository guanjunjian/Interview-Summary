## `deque-operator[]()`

```c++
//调用operator*()和operator+()
reference operator[](difference_type n) const { return *(*this + n); }
```

