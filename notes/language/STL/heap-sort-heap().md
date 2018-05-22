## heap-sort-heap()

- comp传入为小于函数时，不断pop_heap相当于尾部不断堆积最大值，因此最后得到一个递增序列（**默认情况**）
- comp传入为大于函数时，不断pop_heap相当于尾部不断堆积最小值，因此最后得到一个递减序列

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

