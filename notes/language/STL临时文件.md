### 4.4.6 deque的元素操作

- 1.push_back()
- 2.push_front()
- 3.pop_back()
- 4.pop_front()
- 5.clear()：清除整个deque。注意：deque的最初状态（无任何元素时）保有一个缓冲区，因此clear()完之后恢复初始状态，也一样要保留一个缓冲区
- 6.erase()：有两个重载版本，都采用覆盖方法
  - erase(pos)：清除pos迭代器指向的元素，返回原pos之后的元素的迭代器
  - erase(first,last)：清除[first,last)区间内的所有元素，返回原last指向的元素的迭代器
- 7.insert(pos,x)：允许在pos之前插入一个元素x，返回新插入元素的迭代器

[deque的元素操作](STL/deque-元素操作)