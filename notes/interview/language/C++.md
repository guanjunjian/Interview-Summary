C++

# 关键字

**1. extern关键字作用**

-	1).extern声明变量在文件外部定义？
-	2).extern修饰函数？
-	3).extern C的作用？用法？

答：

参考:
-	[C/C++中extern关键字详解](http://blog.csdn.net/big_bit/article/details/51595714)


1)、2):

参考:

-	[《C和指针》笔记 3.10](https://guanjunjian.github.io/2017/01/09/study-19-pointers-on-c-summary/)

在C语言中，修饰符extern用在变量或者函数的声明前，用来说明“此变量/函数是在别处定义的，要在此处引用”。extern声明不是定义，即不分配存储空间。

也就是说，在一个文件中定义了变量和函数， 在其他文件中要使用它们， 可以有两种方式：

-	使用头文件，然后声明它们，然后其他文件去包含头文件
-	在其他文件中直接extern

3):

参考：

-	[「二十三」《C++Primber》笔记 第IV部分 19.8.3](https://guanjunjian.github.io/2017/02/09/study-23-cpp-primer-summary_4/)

为了在C++中使用C

---

**2. static关键字作用**

-	1).static修饰局部变量？
-	2).static全局变量？(限定变量在一个编译单元内，一个编译单元就是指一个cpp和它包含的头文件，这个回答可以结合编译需要经历的几个过程来答)
-	3).static修饰普通函数？
-	4).static修饰成员变量?
-	5).static修饰成员函数？


