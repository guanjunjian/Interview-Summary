#### chunk_alloc()

```c++
//假设size已经适当上调至8的倍数
//注意参数nobjs是pass by reference（引用传递）
template <bool threads, int inst>
char*
__default_alloc_template<threads, inst>::chunk_alloc(size_t size, int& nobjs)
{
    char * result;
    size_t total_bytes = size * nobjs;
    //内存池剩余空间
    size_t bytes_left = end_free - start_free;
     
    if (bytes_left >= total_bytes) {
        //内存池剩余空间完全满足需求量
        result = start_free;
        start_free += total_bytes;
        return(result);
    } else if (bytes_left >= size) {
        //内存池剩余空间不能完全满足需求量，但足够供应一个(含)以上的区块
        //那么能分配多少区块，就分配多少区块
        nobjs = bytes_left/size;
        total_bytes = size * nobjs;
        result = start_free;
        start_free += total_bytes;
        return(result);
    } else {
        //内存池剩余空间连一个区块的大小都无法提供
        //计算需要从heap申请的空间数量，用以补充内存池
        //注意，这里是2倍total_bytes
        size_t bytes_to_get = 2 * total_bytes + ROUND_UP(heap_size >> 4);
        //试着让内存池中的残余零头还有利用价值
        if (bytes_left > 0) {
            //内存池中还有一些零头，先配给适当的free-list
            //首先寻找适当的free-list
            obj * volatile * my_free_list =
                        free_list + FREELIST_INDEX(bytes_left);
            
            //调整free-list，将内存池中的残余空间编入
            ((obj *)start_free) -> free_list_link = *my_free_list;
            *my_free_list = (obj *)start_free;
        }
        //调用malloc，分配heap空间，用来补充内存池
        start_free = (char *)malloc(bytes_to_get);
        if (0 == start_free) {
            //heap空间不足，malloc()失败
            int i;
            obj * volatile * my_free_list, *p;
            //试着检视我们手上拥有的东西。这不会造成伤害。我们不打算尝试分配
            //较小的区块，因为那在多进程机器上容易导致灾难
            //以下搜寻适当的free-list
            //所谓适当是指”尚有未用区块，且区块够大“的free-list
            //在[size,128]这个范围的free list
            for (i = size; i <= __MAX_BYTES; i += __ALIGN) {
                my_free_list = free_list + FREELIST_INDEX(i);
                p = *my_free_list;
                if (0 != p) {
                    //free-list内尚有未用区块
                    //调整free-list以释出未用区块，放入内存池中
                    *my_free_list = p -> free_list_link;
                    start_free = (char *)p;
                    end_free = start_free + i;
                    //递归调用自身，为了修正nobjs
                    return(chunk_alloc(size, nobjs));
                    //注意，任何残余零头终将被编入适当的free-list中备用
                }
            }
            //山穷水尽，到处都没有内存可用 
            end_free = 0;	
            //调用第一级分配器，看看out-of-memory机制能否尽点力
            //可能会抛出异常，或者内存不足的情况获得改善
            start_free = (char *)malloc_alloc::allocate(bytes_to_get);
        }
        heap_size += bytes_to_get;
        end_free = start_free + bytes_to_get;
        //递归调用自身，为了修正nobjs
        return(chunk_alloc(size, nobjs));
    }
}
```

