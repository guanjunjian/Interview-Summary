## RB-tree-插入节点

- 节点值独一无二：insert_unique()
  - 如果树中已存在相同的键值，插入操作就不会真正地进行

- 允许节点值重复：insert_equal()
  - 无论如何插入都会成功，除非空间不足导致配置失败

> insert_equal()

```c++
//插入新值：节点键值允许重复
//注意，返回值是一个RB-tree迭代器，指向新增节点
template <class Key, class Value, class KeyOfValue, class Compare, class Alloc>
typename rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::iterator
rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::insert_equal(const Value& v)
{
  link_type y = header; //y用于保存插入点的父节点
  link_type x = root(); //从根节点开始
  while (x != 0) {
    y = x;
    //“小于”则往左，“大于等于”则往右
    x = key_compare(KeyOfValue()(v), key(x)) ? left(x) : right(x);
  }
  //x为新值插入点，y为插入点的父节点，v为新值
  return __insert(x, y, v);
}
```

> insert_unique()

```c#
//插入新值，节点键值不允许重复，若重复则插入无效
//返回值是个pair，第一个元素是个RB-tree迭代器，指向新增节点
//第二个元素表示插入是否成功
template <class Key, class Value, class KeyOfValue, class Compare, class Alloc>
pair<typename rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::iterator, bool>
rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::insert_unique(const Value& v)
{
  link_type y = header; //y用于保存插入点的父节点
  link_type x = root(); //从根节点开始
  bool comp = true;
  while (x != 0) {
    y = x;
    //v的键值小于目前节点的键值？
    comp = key_compare(KeyOfValue()(v), key(x));
    //“小于”则往左，“大于等于”则往右
    x = comp ? left(x) : right(x);
  }
  iterator j = iterator(y); //构造一个指向节点y的迭代器  
  if (comp) //将插入左侧
    if (j == begin()) //如果插入点的父节点为最左子节点，直接插入，因为键值肯定没重复    
      return pair<iterator,bool>(__insert(x, y, v), true);
    else  //如果插入点的父节点不是最左子节点，说明是BST中间范围的值，可能重复
      --j;  //调整插入点的父节点，找到插入点的前一节点，看是否重复（因为如果重复的话肯定是在[前一节点,y]之间）
  //新键值不与既有节点的键值重复，于是执行安插操作
  if (key_compare(key(j.node), KeyOfValue()(v)))
    return pair<iterator,bool>(__insert(x, y, v), true);
  //到这里表示新值一定与树中键值重复
  return pair<iterator,bool>(j, false);
}
```

> __insert()

```c++
template <class Key, class Value, class KeyOfValue, class Compare, class Alloc>
typename rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::iterator
rb_tree<Key, Value, KeyOfValue, Compare, Alloc>::
__insert(base_ptr x_, base_ptr y_, const Value& v) {
  //x_为新值插入点，y_为插入点的父节点，v为新值
  link_type x = (link_type) x_;
  link_type y = (link_type) y_;
  link_type z;

  //如果新值v的键小于节点y的键，将新值作为y的左孩子
  if (y == header || x != 0 || key_compare(KeyOfValue()(v), key(y))) {
    z = create_node(v);
    left(y) = z;                //  当 y == header 时，leftmost() = z 
    if (y == header) {
      root() = z;
      rightmost() = z;
    }
    else if (y == leftmost())   //如果y为最左节点
      leftmost() = z;           // 维护 leftmost() 指向最左节点
  }
  else { //新值v的键值大于节点y的键，将新值作为y的右孩子
    z = create_node(v);       
    right(y) = z;               //令新节点成为插入点的父节点y的右子节点
    if (y == rightmost())       //如果y为最左节点
      rightmost() = z;          // 维护 rightmost() 指向最右节点
  }
  //设置新节点的指针，没有设置颜色信息
  parent(z) = y;
  left(z) = 0;
  right(z) = 0;
  //进行必要的旋转和颜色变换
  __rb_tree_rebalance(z, header->parent);
  //更新树的大小
  ++node_count;
  return iterator(z);
}
```

> __rb_tree_rebalance()

调整RB-tree：旋转及改变颜色

> __rb_tree_left()

> __rb_tree_right()





