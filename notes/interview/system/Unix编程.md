Unix编程

[TOC]

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

![](../../../pics/interview/system/exit函数.png)

## 3.Linux中创建共享内存的方式

> 创建方式

**方式一**：

![](https://github.com/arkingc/note/raw/master/pic/apue-ipc-19.png) 

`key`：创建IPC结构时需要指定一个键，作为IPC对象的外部名。键由内核转变成标识符

`size`：共享存储段的长度，单位是字节。实现通常将其向上取为系统页长的整倍数。但是，如果指定的值不是系统页长的整倍数，那么最后一页的余下部分是不可使用的

- 如果正在创建一个新段，则必须指定`size`（段内的内容初始化为0） 
- 如果正在引用一个现存的段，则将`size`指定为0 

**方式二**：`mmap`+`/dev/zero`

使用`/dev/zero`的优点为：调用mmap创建映射区之前，无需存在一个实际文件

使用`/dev/zero`的缺点为：只在两个相关进程之间起作用

> 共享内存的存储位置

![](https://github.com/arkingc/note/raw/master/pic/apue-ipc-23.png) 

> 共享内存中起始地址是不是按照页的大小对齐 

长度一般是按页的：共享存储段的长度，单位是字节。实现通常将其向上取为系统页长的整倍数。但是，如果指定的值不是系统页长的整倍数，那么最后一页的余下部分是不可使用的

获取的地址一般可选：

![](https://github.com/arkingc/note/raw/master/pic/apue-ipc-21.png) 

`shmid`：共享存储段的标识符

`addr`：共享存储段连接到进程的该地址 

- `0`：由内核选择（推荐的方式）
- `非0`
  - `flag`指定了`SHM_RND`，则连接到`addr`所指的地址上 
  - `flag`没指定`SHM_RND`，则此段连接到 `addr-(addr mod SHMLAB)` 所表示的地址上（`SHM_RND`意思是”取整“，`SHMLBA`的意思是”低边界地址倍数“） 

> 创建共享内存的时候物理页一定分配吗 

内存映射文件并不是调用mmap的时候就将该文件拷贝到内存中，而是建立逻辑地址到文件地址之间的映射关系，但访问这段内存的数据时还是引发缺页中断，然后将该页的数据换到物理地址上

惰性分配：至到真正需要的时候再分配空间，减少响应时间，如果从来没有用到这个对象的情况下可以减少分配。 

## 3.子进程的生成、子进程的结束

> fork的使用方法 

![](https://github.com/arkingc/note/raw/master/pic/apue-processctr-2.png) 

返回值： 

- **父进程返回子进程ID**：原因是，一个进程的子进程可以有多个，并且没有一个函数使一个进程可以获得其所有子进程的进程ID 
- **子进程返回0**：原因是，子进程总是可以调用`getppid`获得父进程的ID（0号进程总是由内核交换进程使用，所以一个子进程的进程ID不可能为0） 

> 子进程结束以后父进程如何知道 

SIGCHLD信号：子进程终止时给父进程发送的信号，如果父进程没有捕获代码，则这个信号被忽略 

> 父进程在子进程结束以后要做什么事情 

父进程能通过`wait`、`waitpid`获取子进程的终止状态，并回收其进程号

![](https://github.com/arkingc/note/raw/master/pic/apue-processctr-4.png) 

## 4.wait和waitpid的区别

```c++
#include <sys/wait.h>

/*
** 作用：处理已终止的子进程，如果调用wait时没有已终止的子进程，但有仍然在运行的子进程，则wait会阻塞到现有子进程第一个终止为止
** @statloc：子进程终止状态，可以调用三个宏来检查终止状态，并分辨子进程是正常终止、由某个信号杀死还是作业控制停止
** @返回值：已终止子进程的进程ID号
*/
pid_t wait (int *statloc);

/*
** 相比wait，waipid可以选择等待哪个进程，以及是否阻塞等
** @pid：指定想等待的进程ID，-1表示等待第一个终止的子进程
** @statloc：子进程终止状态
** @options：指定附加选项，如WNOHANG表示告知内核再没有已终止子进程时不要阻塞
** @返回值：已终止子进程的进程ID号
*/
pid_t waitpid (pid_t pid, int *statloc, int options);
```

- 可选择等待结束的子进程
- 可选择无可等待子进程时，不阻塞

## 5.互斥锁和条件变量

[再谈互斥锁与条件变量！（终于搞清楚了啊！！！！！）](http://blog.chinaunix.net/uid-27164517-id-3282242.html)

> pthread_cond_boardcase和pthread_cond_signal的区别

激发条件有两种形式，pthread_cond_signal()激活一个等待该条件的线程，存在多个等待线程时按入队顺序激活其中一个；而pthread_cond_broadcast()则激活所有等待线程。 

## 6.互斥锁的唤醒顺序

[使用互斥锁](https://docs.oracle.com/cd/E19253-01/819-7051/6n919hpag/index.html)

缺省调度策略 `SCHED_OTHER` 不指定线程可以获取锁的顺序。如果多个线程正在等待一个互斥锁，则获取顺序是不确定的。出现争用时，缺省行为是按优先级顺序解除线程的阻塞。 

