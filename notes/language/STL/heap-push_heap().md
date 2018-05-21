## heap-push_heap()

**函数执行条件**：

- 1.该函数接受两个迭代器，用来表现一个heap底部容器（vector）的头尾
- 2.新元素插入到底部容器的最尾部（调用push_back()）

如果不符合这两个条件，push_heap的执行结果未可预期

```
push_heap()
	---> __push_heap_aux()
		---> __push_heap()
```

**push_heap()**：

需要传入**比较函数**，让用户指定实现max-heap还是min-heap

该函数还有一个默认使用operator<的版本，因此默认情况下实现的是max-heap

```c++
template <class RandomAccessIterator, class Compare>
inline void push_heap(RandomAccessIterator first, RandomAccessIterator last,
                      Compare comp) {
  //注意，此函数被调用时，新元素应已置于底部容器的最尾端
  __push_heap_aux(first, last, comp, distance_type(first), value_type(first));
}
```

**__push_heap_aux()**:

```c++
template <class RandomAccessIterator, class Compare, class Distance, class T>
inline void __push_heap_aux(RandomAccessIterator first,
                            RandomAccessIterator last, Compare comp,
                            Distance*, T*) {
  //以下系根据“隐式表示堆”的结构特性：新值必须置于底部容器的最尾部
  //此即第一个洞号：(last-first)-1
  __push_heap(first, Distance((last - first) - 1), Distance(0), 
              T(*(last - 1)), comp);
}
```

**__push_heap()**：

默认情况下，comp传入的是小于函数，生成的是max-heap

```c++
template <class RandomAccessIterator, class Distance, class T, class Compare>
void __push_heap(RandomAccessIterator first, Distance holeIndex,
                 Distance topIndex, T value, Compare comp) {
  //找出父节点
  Distance parent = (holeIndex - 1) / 2;
  //尚未达到顶端，且父节点小于新值
  while (holeIndex > topIndex && comp(*(first + parent), value)) {
    //令洞值为父值
    *(first + holeIndex) = *(first + parent);
    //调整洞号，向上提升至父节点
    holeIndex = parent;
    //新洞号的父节点
    parent = (holeIndex - 1) / 2;
  }
  //令洞值为新值，完成插入操作
  *(first + holeIndex) = value;
}
```

