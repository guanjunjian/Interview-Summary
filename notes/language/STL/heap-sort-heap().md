## heap-sort-heap()

```c++
template <class RandomAccessIterator, class Compare>
void sort_heap(RandomAccessIterator first, RandomAccessIterator last,
               Compare comp) {
  //以下，每次执行一次pop_heap()，极值即被放在尾端，
  //扣除尾端再执行一次pop_heap(),次极值又被放在新尾端。
  //每执行pop_heap()一次，操作范围退缩一格
  while (last - first > 1) pop_heap(first, last--, comp);
}
```

