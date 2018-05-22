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
  //如果长度为0或者1，不必重新排序
  if (last - first < 2) return;
  Distance len = last - first;
  //找出第一个需重排的子树头部（最后一个有子节点的节点，最后一个非叶节点），以parent标出，对其执行“下溯程序”，直到根节点
  Distance parent = (len - 2)/2;
    
  while (true) {
    //重排以parent为首的子树。len是为了让__adjust_heap()判断操作范围
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

