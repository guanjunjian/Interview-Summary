## heap-pop_heap()

**函数执行条件**：该函数接受两个迭代器，用来表现一个heap底层容器（vector）的头尾

如果不符合这个条件，pop_heap的执行结果未可预期

```
pop_heap()
	---> __pop_heap()
		---> __adjust_heap()
			---> __adjust_heap()
```

**pop_heap()**：

comp默认传入的是小于函数，因此生成max-heap

```c++
template <class RandomAccessIterator, class Compare>
inline void pop_heap(RandomAccessIterator first, RandomAccessIterator last,
                     Compare comp) {
    __pop_heap_aux(first, last, value_type(first), comp);
}
```

**__pop_heap_aux()**：

```c++
template <class RandomAccessIterator, class T, class Compare>
inline void __pop_heap_aux(RandomAccessIterator first,
                           RandomAccessIterator last, T*, Compare comp) {
  //以下，根据“隐式表示堆”的次序特性，pop操作的结果应为底部容器的
  //第一个元素。因此，首先设定欲调整值为尾值，然后将首值调至
  //尾节点（所以以下将迭代器result设为last-1）。然后重整[first,last-1),
  //使之重新成一个合格的heap
  __pop_heap(first, last - 1, last - 1, T(*(last - 1)), comp,
             distance_type(first));
}
```

**__pop_heap**：

```c++
template <class RandomAccessIterator, class T, class Compare, class Distance>
inline void __pop_heap(RandomAccessIterator first, RandomAccessIterator last,
                       RandomAccessIterator result, T value, Compare comp,
                       Distance*) {
  //设定尾值为首值，于是尾值即为欲求结果
  //可由客户端稍后再以底层容器的pop_back()取出尾值
  *result = *first;
  //以下欲重新调整heap，洞号位0（树的根），欲调整的值为value（原尾值）
  __adjust_heap(first, Distance(0), Distance(last - first), value, comp);
}
```

**__adjust_heap()**：

```c++
template <class RandomAccessIterator, class Distance, class T, class Compare>
void __adjust_heap(RandomAccessIterator first, Distance holeIndex,
                   Distance len, T value, Compare comp) {
  Distance topIndex = holeIndex;
  //洞节点的右子节点
  Distance secondChild = 2 * holeIndex + 2;
  while (secondChild < len) {
    //比较洞节点的左右两个子值，以secondChild代表较大节点
    if (comp(*(first + secondChild), *(first + (secondChild - 1))))
      secondChild--;
    //令较大值为洞值，再令洞号下移至较大节点
    *(first + holeIndex) = *(first + secondChild);
    holeIndex = secondChild;
    //找出新洞节点的右子节点
    secondChild = 2 * (secondChild + 1);
  }
  //没有右子节点，只有左子节点
  if (secondChild == len) {
    //令左子节点为洞值，再令洞号下移至左子节点处
    *(first + holeIndex) = *(first + (secondChild - 1));
    holeIndex = secondChild - 1;
  }
  //此时（可能）尚未满足次序特性，执行一次“上溯程序”操作
  __push_heap(first, holeIndex, topIndex, value, comp);
}
```

