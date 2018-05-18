## vector-push_back()

```c++
void push_back(const T& x) {
  //还有备用空间
  if (finish != end_of_storage) {
    construct(finish, x);
    ++finish;
  }
  else
    //无备用空间
    insert_aux(end(), x);
}

template <class T, class Alloc>
void vector<T, Alloc>::insert_aux(iterator position, const T& x) {
  if (finish != end_of_storage) {
    //还有备用空间
    //在备用空间起始处构造一个元素，并以vector最后一个元素值为其初值
    construct(finish, *(finish - 1));
    ++finish;
    T x_copy = x;
    copy_backward(position, finish - 2, finish - 1);
    *position = x_copy;
  }
  else {
    //已无备用空间
    const size_type old_size = size();
    //配置原则：如果原大小为0，则配置1个元素大小
    //如果原大小不为0，则配置原大小的两倍
    //前半段用来放置原数据，后半段准备用来放置新数据
    const size_type len = old_size != 0 ? 2 * old_size : 1;
    //进行空间配置
    iterator new_start = data_allocator::allocate(len);
    //初始化新的start和finish
    iterator new_finish = new_start;
    __STL_TRY {
      //将原vector的内容拷贝到新vector
      new_finish = uninitialized_copy(start, position, new_start);
      //为新元素设定初值为x
      construct(new_finish, x);
      ++new_finish;
      //由于insert(p,x)也调用该函数
      //因此需要将安插点之后的内容也拷贝过来
      //对于push_back来说，安插点之后没有数据
      new_finish = uninitialized_copy(position, finish, new_finish);
    }
    catch(...) {
      destroy(new_start, new_finish); 
      data_allocator::deallocate(new_start, len);
      throw;
    }
    
    //析构并释放原vector
    destroy(begin(), end());
    deallocate();
    
    //调整迭代器，指向新vector
    start = new_start;
    finish = new_finish;
    end_of_storage = new_start + len;
  }
}
```

