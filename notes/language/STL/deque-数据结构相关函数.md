## deque-数据结构相关函数

```c++
public:                         // Basic accessors
  iterator begin() { return start; }
  iterator end() { return finish; }
  const_iterator begin() const { return start; }
  const_iterator end() const { return finish; }

  reference operator[](size_type n) { 
      //调用__deque_iterator<>::operator[]()
      return start[difference_type(n)]; }

  reference front() { return *start; }
  reference back() {
    iterator tmp = finish;
    --tmp;
    return *tmp;
  }
  const_reference front() const { return *start; }
  const_reference back() const {
    const_iterator tmp = finish;
    --tmp;
    return *tmp;
  }

  size_type size() const { return finish - start;; }
  //size_t(-1)是该size_t能表示的最大值
  size_type max_size() const { return size_type(-1); }
  bool empty() const { return finish == start; }
```

