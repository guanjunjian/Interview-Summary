```c++
//一般而言是线程安全，并且对于空间的运用比较高效
//无“template型别参数”，至于”非型别参数“inst，则完全没派上用场
template <int inst>
class __malloc_alloc_template {

private:
//以下函数将用来处理内存不足的情况
//oom：out of memorys
static void *oom_malloc(size_t);
static void *oom_realloc(void *, size_t);
static void (* __malloc_alloc_oom_handler)();

public:

static void * allocate(size_t n)
{
    //第一级配置器直接使用malloc()
    void *result = malloc(n); 
    //malloc()无法满足要求时，改用oom_malloc()
    if (0 == result) result = oom_malloc(n);
    return result;
}

static void deallocate(void *p, size_t /* n */)
{
    //第一级配置器直接使用free()
    free(p);
}

static void * reallocate(void *p, size_t /* old_sz */, size_t new_sz)
{
    //第一级配置器直接使用realloc()
    void * result = realloc(p, new_sz);
    //realloc()无法满足要求时，改用oom_realloc()
    if (0 == result) result = oom_realloc(p, new_sz);
    return result;
}

//以下仿真C++的set_new_handler()。换句话说，你可以通过它
//指定你自己的out-of-memory handler，即传入的f
//不能直接运用C++ new-handler机制，因为它并非使用::operator new来分配内存
static void (* set_malloc_handler(void (*f)()))()
{
    //保存旧的处理函数
    void (* old)() = __malloc_alloc_oom_handler;
    __malloc_alloc_oom_handler = f;
    return(old);
}
};


//malloc_alloc out-of-memory handling
//初值设为0，有待客端设定
//设计“内存不足处理例程”是客端的责任
template <int inst>
void (* __malloc_alloc_template<inst>::__malloc_alloc_oom_handler)() = 0;

template <int inst>
void * __malloc_alloc_template<inst>::oom_malloc(size_t n)
{
    void (* my_malloc_handler)();
    void *result;

    for (;;) {
        //不断尝试释放、配置、再释放、再配置……
        my_malloc_handler = __malloc_alloc_oom_handler;
        if (0 == my_malloc_handler) { __THROW_BAD_ALLOC; }
        //调用处理例程，企图释放内存
        (*my_malloc_handler)();
        //再次尝试尝试配置内存
        result = malloc(n);
        if (result) return(result);
    }
}

template <int inst>
void * __malloc_alloc_template<inst>::oom_realloc(void *p, size_t n)
{
    void (* my_malloc_handler)();
    void *result;

    for (;;) {
        //不断尝试释放、配置、再释放、再配置……
        my_malloc_handler = __malloc_alloc_oom_handler;
        if (0 == my_malloc_handler) { __THROW_BAD_ALLOC; }
        //调用处理例程，企图释放内存
        (*my_malloc_handler)();
        //再次尝试尝试配置内存
        result = realloc(p, n);
        if (result) return(result);
    }
}

//注意，以下直接将参数inst指定为0
typedef __malloc_alloc_template<0> malloc_alloc;
```

