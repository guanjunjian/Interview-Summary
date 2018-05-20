## list-insert()

insert是一个重载函数，有多种形式，其中最简单的如下

```c++
  //函数目的：在迭代器position所指位置之前插入一个节点，内容为x
  iterator insert(iterator position, const T& x) {
    //产生一个节点，将内容设为x
    link_type tmp = create_node(x);
    tmp->next = position.node;
    tmp->prev = position.node->prev;
    (link_type(position.node->prev))->next = tmp;
    position.node->prev = tmp;
    return tmp;
  }
```

