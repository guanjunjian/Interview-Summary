## list-destroy_node().md

```c++
  void destroy_node(link_type p) {
    //先析构该节点
    destroy(&p->data);
    //再讲该节点释放
    put_node(p);
  }

```