## list-splice()

> 重载1：splice(iterator position, list& x)

```c++
//将一个list中的所有元素移动到另一个（或同一个）list的某个定点
  void splice(iterator position, list& x) {
    if (!x.empty()) 
      transfer(position, x.begin(), x.end());
  }
```

> 重载2：splice(iterator position, list&, iterator i)

```c++
  //将i所指元素接合于position所指位置之前。position和i可能指向同一个list
  void splice(iterator position, list&, iterator i) {
    iterator j = i;
    ++j;
    //如果i就是position，或者i本来就在position之前，则什么也不做
    if (position == i || position == j) return;
    //否则，调用transfer将i置于position之前
    transfer(position, i, j);
  }
```

> 重载3：splice(iterator position, list&, iterator first, iterator last)

```c++
  //将[first,last)内的所有元素接合于position所指位置之前
  //position和[first,last)可能指向同一个list
  //但position不能位于[first,last)之内
  void splice(iterator position, list&, iterator first, iterator last) {
    if (first != last) 
      transfer(position, first, last);
  }
```

