## hashtable-insert_unique()

```c++
pair<iterator, bool> insert_unique(const value_type& obj)
  {
    resize(num_elements + 1);  //判断是否需要重建表格，如需要就扩充
    return insert_unique_noresize(obj);
}

template <class V, class K, class HF, class Ex, class Eq, class A>
pair<typename hashtable<V, K, HF, Ex, Eq, A>::iterator, bool> 
hashtable<V, K, HF, Ex, Eq, A>::insert_unique_noresize(const value_type& obj)
{
  const size_type n = bkt_num(obj);  //计算obj应位于哪个bucket
  node* first = buckets[n];          //获取指针bucket头结点的指针

  //遍历到节点串查找是否存在键值重复的节点
  for (node* cur = first; cur; cur = cur->next) 
    //如果中途找到了一个键值相同的节点，那么直接返回
    if (equals(get_key(cur->val), get_key(obj)))
      return pair<iterator, bool>(iterator(cur, this), false);

  //根据obj构造新节点
  node* tmp = new_node(obj);
  //将新节点插入串首
  tmp->next = first;
  buckets[n] = tmp;
  //更新元素个数并返回
  ++num_elements;
  return pair<iterator, bool>(iterator(tmp, this), true);
}
```

