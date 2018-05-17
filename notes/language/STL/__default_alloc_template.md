```c++
  enum {__ALIGN = 8};  //小型块区的上调边界
  enum {__MAX_BYTES = 128};  //小型区块的上限
  enum {__NFREELISTS = __MAX_BYTES/__ALIGN};  //free-lists的个数，16种（8、16、24...、128）

//注意，无”template型别参数“，且第二参数完全没派上用场
//第一个参数用于多线程环境，本书不讨论多线程
template <bool threads, int inst>
class __default_alloc_template {

private:
  // ROUND_UP()将bytes上调至8的倍数	
  static size_t ROUND_UP(size_t bytes) {
        // (bytes + 7) & ~7
        return (((bytes) + __ALIGN-1) & ~(__ALIGN - 1));
  }
private:
  union obj {    //free-lists的节点结构
        union obj * free_list_link;
        char client_data[1];    /* The client sees this.        */
  };
private:
  //16个free-lists的声明
  static obj * __VOLATILE free_list[__NFREELISTS]; 
  //根据区块大小，决定使用第n号free-list。n从0算起
  static  size_t FREELIST_INDEX(size_t bytes) {
        return (((bytes) + __ALIGN-1)/__ALIGN - 1);
  }

  //返回一个大小为n的对象，并可能加入大小为n的其它区块到free-list
  static void *refill(size_t n);
  //分配一大块空间，可容纳nobjs个大小为“size”的区块
  //如果分配nobjs个区块有所不便，nobjs可能会降低
  static char *chunk_alloc(size_t size, int &nobjs);

  // Chunk allocation state.（Chunk配置状态）
  static char *start_free; //内存池起始位置。只在chunk_alloc()中变化
  static char *end_free; //内存池结束位置。只在chunk_alloc()中变化
  static size_t heap_size;

public:
  static void * allocate(size_t n){ /*详述于后*/ }
  static void deallocate(void *p, size_t n){ /*详述于后*/ }
  static void * reallocate(void *p, size_t old_sz, size_t new_sz);
} ;


//以下是静态数据成员的定义与初始值*/
template <bool threads, int inst>
char *__default_alloc_template<threads, inst>::start_free = 0;

template <bool threads, int inst>
char *__default_alloc_template<threads, inst>::end_free = 0;

template <bool threads, int inst>
size_t __default_alloc_template<threads, inst>::heap_size = 0;

template <bool threads, int inst>
__default_alloc_template<threads, inst>::obj * volatile
__default_alloc_template<threads, inst> ::free_list[__NFREELISTS] = 
    {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, };
```

