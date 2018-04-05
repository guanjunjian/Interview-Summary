Unix编程

[TOC]

# Linux API

## 1.fork、vfork

[fork()、vfork()、clone()的区别](https://blog.csdn.net/gogokongyin/article/details/51178257)

这三个函数分别调用了sys_fork、sys_vfork、sys_clone，最终都调用了do_fork函数。

进程的四要素

- 程序代码（不一定是一个进程所专有的）
- 专用系统堆栈空间
- 进程控制块
- 专用的用户空间（地址空间）

> 1).fork()

fork()函数调用成功：返回两个值； 父进程：返回子进程的PID；子进程：返回0；失败：返回-1；

fork 创造的子进程复制了父亲进程的资源（写时复制技术），包括进程四要素。

开始时，两者的虚拟空间不同，但其对应的物理空间是同一个。有需要时才写时复制。

一般子进程先调度，但子父线程自由执行。

> 2).vfork

vfork也是创建一个子进程，但是子进程共享父进程的空间。在vfork创建子进程之后，父进程阻塞，直到子进程执行了exec()或者exit()。 vfork是一个过时的应用,vfork最初是因为fork没有实现COW机制，而很多情况下fork之后会紧接着exec，而exec的执行相当于之前fork复制的空间全部变成了无用功，所以设计了vfork。现在fork使用了COW机制，唯一的代价仅仅是复制父进程页表的代价，所以vfork不应该出现在新的代码之中。

进程阻塞，直到子进程执行了exec()或者exit()

缺少**“专用的用户空间”**

## 2.exit()与_exit()区别

exit()清理后进入内核，_exit()直接陷入内核。



