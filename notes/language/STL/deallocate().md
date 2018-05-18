## deallocate()

```c++
  //p不可以是0
  static void deallocate()(void *p, size_t n)
  {
    obj *q = (obj *)p;
    obj * volatile * my_free_list;
    
    //大于128就调用第一级配置器
    if (n > (size_t) __MAX_BYTES) {
        malloc_alloc::deallocate(p, n);
        return;
    }
    //寻找对应的free-list
    my_free_list = free_list + FREELIST_INDEX(n);
    //调整free-list，回收块区，放于链表头部
    q -> free_list_link = *my_free_list;
    *my_free_list = q;
  }
```

