STL源码剖析 笔记

[TOC]

# 第1章 STL概论与版本简介

## 1.2 STL 六大组件 功能与运用

STL提供**六大组件**，彼此可以组合套用： 

- 1.**容器**（containers）：各种数据结构，如：vector、list、deque、set、map。用来存放数据。从实现的角度来看，STL容器是一种**class template** 
- 2.**算法**（algorithms）：各种常用算法，如：sort、search、copy、erase。从实现的角度来看，STL算法是一种 **function template**
- 3.**迭代器**（iterators）：容器与算法之间的胶合剂，是所谓的“泛型指针”。共有五种类型，以及其他衍生变化。从实现的角度来看，迭代器是一种将 `operator*`、`operator->`、`operator++`、`operator- -` 等指针相关操作进行重载的**class template**。所有STL容器都有自己专属的迭代器，只有容器本身才知道如何遍历自己的元素。原生指针(native pointer，就是普通指针)也是一种迭代器
- 4.**仿函数**（functors）：行为类似函数，可作为算法的某种策略（policy）。从实现的角度来看，仿函数是一种重载了operator()的class或**class template**。一般的函数指针也可视为狭义的仿函数
- 5.**配接器**（adapters）：一种用来修饰容器、仿函数、迭代器接口的东西。例如：STL提供的queue 和 stack，虽然看似容器，但其实只能算是一种容器配接器，因为它们的底部完全借助deque，所有操作都由底层的deque供应。
  - 改变 functors接口者，称为function adapter
  - 改变 container 接口者，称为container adapter
  - 改变iterator接口者，称为iterator adapter
- 6.**配置器**（allocators）：负责空间配置与管理。从实现的角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的**class template**

![](../../pics/language/STL源码剖析/img-1-1-STL六大组件的交互关系.png)