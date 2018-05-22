## heap-make_heap()

```
make_heap()
	---> __make_heap()
		---> __adjust_heap()
```

**make_heap()**：

```c++
template <class RandomAccessIterator, class Compare>
inline void make_heap(RandomAccessIterator first, RandomAccessIterator last,
                      Compare comp) {
  __make_heap(first, last, comp, value_type(first), distance_type(first));
}
```

**__make_heap()**：

```c++
template <class RandomAccessIterator, class Compare, class T, class Distance>
void __make_heap(RandomAccessIterator first, RandomAccessIterator last,
                 Compare comp, T*, Distance*) {
  
  if (last - first < 2) return;
  Distance len = last - first;
  Distance parent = (len - 2)/2;
    
  while (true) {
    __adjust_heap(first, parent, len, T(*(first + parent)), comp);
    if (parent == 0) return;
    parent--;
  }
}
```

**__adjust_heap**：

```c++
template <class RandomAccessIterator, class Distance, class T, class Compare>
void __adjust_heap(RandomAccessIterator first, Distance holeIndex,
                   Distance len, T value, Compare comp) {
  Distance topIndex = holeIndex;
  Distance secondChild = 2 * holeIndex + 2;
  while (secondChild < len) {
    if (comp(*(first + secondChild), *(first + (secondChild - 1))))
      secondChild--;
    *(first + holeIndex) = *(first + secondChild);
    holeIndex = secondChild;
    secondChild = 2 * (secondChild + 1);
  }
  if (secondChild == len) {
    *(first + holeIndex) = *(first + (secondChild - 1));
    holeIndex = secondChild - 1;
  }
  __push_heap(first, holeIndex, topIndex, value, comp);
}
```

