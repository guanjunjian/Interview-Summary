## vector-insert()

`insert(position, n, x);`

```c++
template <class T, class Alloc>
void vector<T, Alloc>::insert(iterator position, size_type n, const T& x) {
  //当n不等于0时，才进行以下所有操作
  if (n != 0) {
    //(1)如果备用空间大于等于“新增元素个数”
    if (size_type(end_of_storage - finish) >= n) {
      T x_copy = x;
      //计算插入点之后的现有元素个数
      const size_type elems_after = finish - position;
      iterator old_finish = finish;
      //(1-1)如果“插入点之后的现有元素个数”大于“新增元素个数”
      if (elems_after > n) {
        //uninitialized_copy三个参数表示：源起始、源终止、目的起始
        //1.先将“插入点之后的现有元素”的后n个移动到finish之后
        uninitialized_copy(finish - n, finish, finish);
        //2.更新finish
        finish += n;
        //3.再将“插入点之后的现有元素”剩下的前几个元素向后移动
        copy_backward(position, old_finish - n, old_finish);
        //4.将新加入的元素从“插入点”填入
        fill(position, position + n, x_copy);
      }
      else { //(1-2)“插入点后的现有元素个数”小于等于“新增元素个数”
        //1.在“现有元素”之后先添加“新增元素个数”比“插入点后的现有元素个数”多的数 
        uninitialized_fill_n(finish, n - elems_after, x_copy);
        //2.更新finish
        finish += n - elems_after;
        //3.将“插入点后的现有元素”全部拷贝到新finish之后
        uninitialized_copy(position, old_finish, finish);
        //4.更新finish
        finish += elems_after;
        //5.填入剩余“新增元素个数”
        fill(position, old_finish, x_copy);
      }
    }
    else { //(2)备用空间小于“新增元素个数”（那就必须配置额外的内存）
      //首先决定新长度：旧长度的两倍，或旧长度+新增元素个数
      const size_type old_size = size();        
      const size_type len = old_size + max(old_size, n);
      //1.配置新的vector空间
      iterator new_start = data_allocator::allocate(len);
      iterator new_finish = new_start;
      __STL_TRY {
        //2.首先将旧vector的插入点之前的元素复制到新空间
        new_finish = uninitialized_copy(start, position, new_start);
        //3.再将新增元素（初值皆为n）填入新空间
        new_finish = uninitialized_fill_n(new_finish, n, x);
        //4.最后将旧vector的插入点之后的元素赋值到新空间
        new_finish = uninitialized_copy(position, finish, new_finish);
      }
#         ifdef  __STL_USE_EXCEPTIONS 
      catch(...) {
        //如果有异常发生，实现“commit or rollback”语意
        destroy(new_start, new_finish);
        data_allocator::deallocate(new_start, len);
        throw;
      }
#         endif /* __STL_USE_EXCEPTIONS */
      //若新空间分配成功，清除并释放旧vector空间
      destroy(start, finish);
      deallocate();
      //更新start、finish
      start = new_start;
      finish = new_finish;
      end_of_storage = new_start + len;
    }
  }
}
```

