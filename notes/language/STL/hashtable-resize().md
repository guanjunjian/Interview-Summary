## hashtable-resize()

```c++
template <class V, class K, class HF, class Ex, class Eq, class A>
void hashtable<V, K, HF, Ex, Eq, A>::resize(size_type num_elements_hint)
{
  const size_type old_n = buckets.size();  //bucket的个数
  if (num_elements_hint > old_n) {//如果元素大于bucket的数量，就进行重新分配
    const size_type n = next_size(num_elements_hint); //找出下一个质数
    if (n > old_n) {
      vector<node*, A> tmp(n, (node*) 0);
      __STL_TRY {
        //处理每一个旧的bucket
        for (size_type bucket = 0; bucket < old_n; ++bucket) {
          node* first = buckets[bucket];//指向bucket中list的起始节点
          while (first) {//起始节点不为空
            //找到节点对应新的bucket号
            size_type new_bucket = bkt_num(first->val, n);
            //令目前处理的旧bucket起始节点为first的下一个节点
            buckets[bucket] = first->next;
            //将当前节点链入新的bucket的list中
            first->next = tmp[new_bucket];
            tmp[new_bucket] = first;
            //继续处理旧bucket的下一个节点
            first = buckets[bucket];          
          }
        }
        buckets.swap(tmp); //新旧两个buckets对调
      }
#         ifdef __STL_USE_EXCEPTIONS
      catch(...) {
        for (size_type bucket = 0; bucket < tmp.size(); ++bucket) {
          while (tmp[bucket]) {
            node* next = tmp[bucket]->next;
            delete_node(tmp[bucket]);
            tmp[bucket] = next;
          }
        }
        throw;
      }
#         endif /* __STL_USE_EXCEPTIONS */
    }
  }
}
```

