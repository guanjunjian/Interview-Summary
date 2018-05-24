# 第5章 关联式容器

![](../../pics/language/STL源码剖析/img-4-1.png)

以**RB-tree**作为底层机制（自身也是关联容器，但不对外开放）

- 1.set
- 2.multiset
- 3.map
- 4.multimap

以**hash table**作为底层机制（自身也是容器，由SGI STL提供，不在标准规范之列，下面几个容器也不在）

- 1.hash_set
- 2.hash_multiset
- 3.hash_map
- 4.hash_multimap

**定义**：每个元素都有一个键值（key）和一个实值（value）。当元素被插入到关联式容器中时，容器内部结构（可能是RB-tree，也可能是hash-table）便依照其键值大小，以某种特定规则将这个元素放置于适当的位置。

**特性**：关联容器没有所谓头尾（只有最大元素和最小元素），所以不会有以下操作：

- 1.push_back()
- 2.push_frornt()
- 3.pop_back()
- 4.pop_front()
- 5.begin()
- 6.end()

**实现**：关联式容器的内部结构是一个平衡二叉树，被广泛运用于STL的是RB-tree（红黑树）

## 5.1 树的导览

**术语**：

- 1.路径长度：根节点至任何节点之间有唯一路径，路径所经过的边数
- 2.深度：根节点至任一节点的路径长度，即该节点的深度。根节点的深度为0
- 3.高度：某节点至其最深子节点（叶节点）的路径长度。树的高度即是根节点的高度
- 4.大小：任何节点的大小时指其所有子代（包括自己）的节点总数

![](../../pics/language/STL源码剖析/img-5-2.png)

> 二叉搜索树

**插入**：

![](../../pics/language/STL源码剖析/img-5-5.png)

**删除1：只有一个子节点**：

![](../../pics/language/STL源码剖析/img-5-6a.png)

**删除2：有两个子节点**：

![](../../pics/language/STL源码剖析/img-5-6b.png)

> 平衡二叉搜索树

“平衡”的大致意思是：没有任何一个节点深度过大

AVL-tree、RB-tree、AA-tree均可实现平衡二叉搜索树

> AVL tree

**AVL tree要求**：任何节点的左右子树高度相差最多维1

**插入后不平衡的情况**：

![](../../pics/language/STL源码剖析/img-5-8.png)

**调整**：只要调整“插入点至根节点”路径上，平衡状态被破坏的各个节点中最深的那一个，便可使整棵树重新获得平衡

**“平衡被破坏”的四种分类**：（假设最深节点为X，“平衡被破坏”意味着X的左右两颗子树的高度相差2）

- 1.插入点位于X的左子节点的左子树---左左（与4对称，**外侧插入**）
- 2.插入点位于X的左子节点的右子树---左右（与3对称，**内侧插入**）
- 3.插入点位于X的右子节点的左子树---右左（与2对称，**内侧插入**）
- 4.插入点位于X的右子节点的右子树---右右（与1对称，**外侧插入**）

外侧插入解决办法：单旋转操作

内侧插入解决办法：双旋转操作

![](../../pics/language/STL源码剖析/img-5-9.png)

**单旋转**：

![](../../pics/language/STL源码剖析/img-5-10.png)

**双旋转**：

![](../../pics/language/STL源码剖析/img-5-12.png)

## 5.2 RB-tree

RB-tree（红黑树）是一个二叉搜索树，并且满足下图的4个条件：

![](../../pics/language/STL源码剖析/img-5-13.png)

**另**：新增节点必须为红色

### 5.2.1 插入节点

![](../../pics/language/STL源码剖析/img-5-14.png)

为了方便讨论，为某些特殊节点定义一些代名词

```
X:新节点
P：父节点
G：祖父节点
S：伯父节点，父节点的兄弟节点
GG：曾祖父节点

   GG
   /
   G
  / \
  P  S
 /
X
```

根据X的插入位置及外围节点（S和GG）的颜色，有四种考虑：

- 情况1：S为黑色，X为外侧插入
- 情况2：S为黑色，X为内侧插入
- 情况3：S为红色，X为外侧插入，GG为黑色
- 情况4：S为红色，X为外侧插入，GG为红色

> 情况1：S为黑色，X为外侧插入

**解决方法**：

- a：P，G做单旋转
- b：更改P，G的颜色

此时可能产生不平衡状态（高度相差1以上），这是可以的，因为RB-tree没有这么严格

![](../../pics/language/STL源码剖析/img-5-15a.png)

> 情况2：S为黑色，X为内侧插入

**解决方法**：

- a：P，X做单旋转
- b：更改G，X颜色
- c：G左单旋转

![](../../pics/language/STL源码剖析/img-5-15b.png)

> 情况3：S为红色，X为外侧插入，GG为黑色

**解决方法**：

- a：P，G做单旋转，
- b：改变X的颜色

![](../../pics/language/STL源码剖析/img-5-15c.png)

> 情况4：S为红色，X为外侧插入，GG为红色

**解决方法**：

- a：P，G做单旋转
- b：改变X的颜色
- c：持续往上做，直到不再有父子连续为红的情况

![](../../pics/language/STL源码剖析/img-5-15d.png)

### 5.2.2 一个由上而下的程序

**目的**：为了避免**情况4**”父子节点皆为红色“的情况持续向RB-tree的上层结构发展，形成处理时效上的瓶颈，可以施行一个**由上而下的程序**

**由上而下的程序**：假设新增节点A，那么沿着A的路径向上，只要看到有某个节点X的两个子节点皆为红色，就把X改为红色，并把两个子节点改为黑色

![](../../pics/language/STL源码剖析/img-5-15e.png)

**插入的操作**：

- 1.如果插入节点A的父节点P仍然为红色（此时S不可能为红），则根据情况1或情况2处理
  - 情况1：S为黑色，A为外侧插入
  - 情况2：S为黑色，A为内侧插入
- 2.如果插入节点A的父节点P不为红色
  - 要么，直接插入
  - 要么，插入，再做一次单旋转（如下图）

![](../../pics/language/STL源码剖析/img-5-15f.png)

### 5.2.3 RB-tree的节点设计

为了有更大的弹性，节点分为两层：`__rb_tree_node`继承于`__rb_tree_node_base`

minimum()和maximum()函数可以让RB-tree搜索极值变得很容易

RB-tree的各种操作时常需要上溯其父节点,因此有parent指针

```c++
typedef bool __rb_tree_color_type;
const __rb_tree_color_type __rb_tree_red = false;   //红色为0
const __rb_tree_color_type __rb_tree_black = true;  //黑色为1

//RB-tree节点的基类
struct __rb_tree_node_base
{
  typedef __rb_tree_color_type color_type;
  typedef __rb_tree_node_base* base_ptr;

  color_type color; //颜色
  base_ptr parent;  //指向父节点的指针
  base_ptr left;    //指向左子节点的指针
  base_ptr right;   //指向右子节点的指针

  //静态函数，获取以x为根节点的RB-tree最小节点的指针
  static base_ptr minimum(base_ptr x)
  {
    while (x->left != 0) x = x->left;
    return x;
  }

  //静态函数，获取以x为根节点的RB-tree最大节点的指针
  static base_ptr maximum(base_ptr x)
  {
    while (x->right != 0) x = x->right;
    return x;
  }
};

//RB-tree节点类
template <class Value>
struct __rb_tree_node : public __rb_tree_node_base
{
  typedef __rb_tree_node<Value>* link_type;
  Value value_field;    //RB-tree节点的value
};
```

**键和值都包含在value_field中** 

下面是RB-tree的节点图表，其中`__rb_tree_node::value_field`填为10

![](../../pics/language/STL源码剖析/img-5-15f-后.png)

### 5.2.4 RB-tree的迭代器

RB-tree迭代器实现为两层：`__rb_tree_iterator`继承于`__rb_tree_base_iterator`

![](../../pics/language/STL源码剖析/img-5-16.png)

RB-tree迭代器属于双向迭代器，但不具备随机定位能力 

前进操作operator++()调用了基类迭代器的increment()，后退操作operator--()调用了基类迭代器的decrement()。前进或后退的举止行为完全依据二叉搜索树的节点排列法则 

**__rb_tree_base_iterator**：

```c++
//迭代器基类
struct __rb_tree_base_iterator
{
  typedef __rb_tree_node_base::base_ptr base_ptr;
  typedef bidirectional_iterator_tag iterator_category;
  typedef ptrdiff_t difference_type;

  base_ptr node;    //节点基类类型的指针，将迭代器连接到RB-tree的节点

  //实际上就是中序遍历的“下一个节点”问题，
  //但需要考虑RB-tree头节点的特殊情况，参考状况（4）
  void increment()
  {
    //状况（1）：如果node右子树不为空，则找到右子树的最左子节点
    if (node->right != 0) {
      node = node->right;
      while (node->left != 0)
        node = node->left;
    }
    else {
      //状况（2）：如果node右子树为空，
      //需要找到某个节点，该节点是其父节点的左孩子
      base_ptr y = node->parent;
      while (node == y->right) {
        node = y;
        y = y->parent;
      }
      //状况（3）：找到了某个节点node，该节点是其父节点y的左孩子，
      //如果node的右孩子不是其父节点y，
      //则increment的结果为找到节点node的父节点y
      if (node->right != y)
        node = y;
      //状况（4）：找到了某个节点node，该节点是其父节点y的左孩子，
      //如果node的右孩子是其父节点y，
      //则increment的结果为找到节点node，
      //当迭代器指向根节点，而根节点无右子节点时，对迭代器进行increment，
      //会进入状况（4），结果为end()，详情看下图
    }
  }
  
  //实际上就是中序遍历的“上一个节点”问题，
  //但需要考虑RB-tree头节点的特殊情况，参考状况（1）
  void decrement()
  {
    //状况（1）：这种情况发生于node为header时（亦即node为end()时）
    //header右子节点即mostright，指向max节点
    //当迭代器为end()时，若对它进行decrement，会进入状态（1），结果为max
    if (node->color == __rb_tree_red &&
        node->parent->parent == node)
      node = node->right;            //
    else if (node->left != 0) {
     //状况（2）：如果左子树不为空，则找到左子树的最右子节点
      base_ptr y = node->left;
      while (y->right != 0)
        y = y->right;
      node = y;
    }
    else {
      //状况（3）：如果左子树为空，则到某个节点node，该节点node是其父节点y的右孩子，
      //则decrement的结果为该节点的父节点y
      base_ptr y = node->parent;
      while (node == y->left) {
        node = y;
        y = y->parent;
      }
      node = y;
    }
  }
};
```

![](../../pics/language/STL源码剖析/img-5-17.png)

**__rb_tree_iterator**：

```c++
//迭代器类
template <class Value, class Ref, class Ptr>
struct __rb_tree_iterator : public __rb_tree_base_iterator
{
  typedef Value value_type;
  typedef Ref reference;
  typedef Ptr pointer;
  typedef __rb_tree_iterator<Value, Value&, Value*>             iterator;
  typedef __rb_tree_iterator<Value, const Value&, const Value*> const_iterator;
  typedef __rb_tree_iterator<Value, Ref, Ptr>                   self;
  typedef __rb_tree_node<Value>* link_type; //指向RB-tree节点的指针类型

  __rb_tree_iterator() {}
  __rb_tree_iterator(link_type x) { node = x; }
  __rb_tree_iterator(const iterator& it) { node = it.node; }

  //解引用操作为获取所指RB-tree节点的value
  reference operator*() const { return link_type(node)->value_field; }
#ifndef __SGI_STL_NO_ARROW_OPERATOR
  pointer operator->() const { return &(operator*()); }
#endif /* __SGI_STL_NO_ARROW_OPERATOR */

  //调用父类的increment()，函数会修改node成员，使其指向后一个RB-tree节点
  self& operator++() { increment(); return *this; }
  self operator++(int) {
    self tmp = *this;
    increment();
    return tmp;
  }
    
  //调用父类的decrement()，函数会修改node成员，使其指向前一个RB-tree节点
  self& operator--() { decrement(); return *this; }
  self operator--(int) {
    self tmp = *this;
    decrement();
    return tmp;
  }
};
```

### 5.2.5 RB-tree的数据结构

```c++
class rb_tree {
protected:
  typedef void* void_pointer;
  typedef __rb_tree_node_base* base_ptr;
  typedef __rb_tree_node<Value> rb_tree_node;
  typedef simple_alloc<rb_tree_node, Alloc> rb_tree_node_allocator;
  typedef __rb_tree_color_type color_type;
public:
  typedef Key key_type;
  typedef Value value_type;
  typedef value_type* pointer;
  typedef const value_type* const_pointer;
  typedef value_type& reference;
  typedef const value_type& const_reference;
  typedef rb_tree_node* link_type;
  typedef size_t size_type;
  typedef ptrdiff_t difference_type;
  //...
protected:
  size_type node_count; // keeps track of size of tree
  link_type header;  
  Compare key_compare;
  //...
public:
  typedef __rb_tree_iterator<value_type, reference, pointer> iterator;
  //...
};
```

### 5.2.6 RB-tree的构造



### 5.2.7 RB-tree的元素操作

- **节点操作**
  - [涉及内存管理的操作](STL/RB-tree涉及内存管理的操作.md)
    - 分配节点：get_node()
    - 释放节点：put_node()
    - 创建节点：create_node()
    - 拷贝节点：clone_node()
    - 销毁节点：destroy_node()
  - [获取节点成员的操作](STL/RB-tree_获取节点成员的操作.md)
    - left()
    - right()
    - parent()
    - value()
    - key()
    - color()
- **RB-tree操作** 
  - 创建空RB-tree：rb_tree()
    - 初始化：init()
  - 获取root节点：root()
  - 获取最左子节点：leftmost()
  - 获取最右子节点：rightmost()
  - 获取起始节点：begin()
  - 获取末尾节点：end()
  - 是否为空：empty()
  - 大小：size()
  - **插入节点**：
    - 节点值独一无二：insert_unique()
    - 允许节点值重复：insert_equal()
  - **元素搜索**： 
    - find()

