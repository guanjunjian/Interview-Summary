STL

# 内存

## 1.STL内存池的实现

STL内存分配分为一级分配器和二级分配器，一级分配器就是采用malloc分配内存，二级分配器采用内存池。

二级分配器设计的非常巧妙，分别给8k，16k,…, 128k等比较小的内存片都维持一个空闲链表，每个链表的头节点由一个数组来维护。需要分配内存时从合适大小的链表中取一块下来。假设需要分配一块10K的内存，那么就找到最小的大于等于10k的块，也就是16K，从16K的空闲链表里取出一个用于分配。释放该块内存时，将内存节点归还给链表。

如果要分配的内存大于128K则直接调用一级分配器。 

为了节省维持链表的开销，采用了一个union结构体，分配器使用union里的next指针来指向下一个节点，而用户则使用union的空指针来表示该节点的地址。

# 容器

## 1.STL里set和map是基于什么实现的

> 1).STL里set和map是基于什么实现的

set和map都是基于红黑树实现的。

> 2).红黑树的特点？

红黑树是一种平衡二叉查找树，与AVL树（自平衡二叉查找树）的区别是：AVL树是完全平衡的，红黑树基本上是平衡的。

> 3).为什么选用红黑数？

因为红黑数是平衡二叉树，其插入和删除的效率都是N(logN)，与AVL相比红黑数插入和删除最多只需要3次旋转，而AVL树为了维持其完全平衡性，在坏的情况下要旋转的次数太多。

> 4).红黑树的定义

-	a.节点是红色或者黑色
-	b.父节点是红色的话，子节点就不能为红色
-	c.从根节点到每个叶子节点路径上黑色节点的数量相同
-	d.根是黑色的，NULL节点被认为是黑色的

# 库函数

//TODO

## 1.手写strcpy

```c
char* strcpy(char* dst, const char* src)
{
    assert(dst);
    assert(src);
    char* ret = dst;
    while((*dst++ = *src++) != '\0');
    return ret;
}
//该函数是没有考虑重叠的

char* strcpy(char* dst, const char* src)
{
    assert((dst != NULL) && (src != NULL));
    char* ret = dst;
    int size = strlen(src) + 1;
    if(dst > src || dst < src + len)
    {
        dst = dst + size - 1;
        src = src + size - 1;
        while(size--)
        {
            *dst-- = *src--;
        }
    }
    else
    {
        while(size--)
        {
            *dst++ = *src++;
        }
    }
    return ret;
}
```

## 2.手写memcpy函数

```c
void* memcpy(void* dst, const void* src, size_t size)
{
    if(dst == NULL || src == NULL)
    {
        return NULL;
    }
    void* res = dst;
    char* pdst = (char*)dst;
    char* psrc = (char*)src;

    if(pdst > psrc && pdst < psrc + size) //重叠
    {
        pdst = pdst + size - 1;
        psrc = pdst + size - 1;
        while(size--)
        {
            *pdst-- = *psrc--;
        }
    }
    else //无重叠
    {
        while(size--)
        {
            *dst++ = *src++;
        }
    }
    return ret;
}
```

## 3.手写strcat函数

```c
char* strcat(char* dst, const char* src)
{
    char* ret = dst;

    while(*dst != '\0')
        ++dst;

    while((*dst++ = *src) != '\0');
    return ret;
}
```

## 4.手写strcmp函数

```c
int strcmp(const char* str1, const char* str2)
{

    while(*str1 == *str2 && *str1 != '\0')
    {
        ++str1;
        ++str2;
    }
    return *str1 - *str2;
}
```
