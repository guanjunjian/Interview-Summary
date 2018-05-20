## list-list()

> 构造函数

list提供许多构造函数，其中一个是默认构造函数，允许我们不指定任何参数做出一个空的list

```c++
public:
list() { empty_initialize(); }

protected:
  void empty_initialize() { 
    node = get_node();
    node->next = node;
    node->prev = node;
  }
```