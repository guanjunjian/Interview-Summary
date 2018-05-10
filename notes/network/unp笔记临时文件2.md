## 7.11 fcntl函数

fcntl函数名字实际意义是“file control”（文件控制），该函数可以执行各种描述符控制操作

```c
#include <fcntl.h>

int fcntl(int fd, int cmd, ... /* int arg */ );

//返回值：若成功则取决于cmd，若出错则为-1
```

- **fd**：文件描述

- **cmd**：作用于该描述符上的命令 

  - **F_GETFL**：获取文件标志（影响套接字描述符的两个标志：O_NONBLOCK（非阻塞式I/O）、O_ASYNC（信号驱动式I/O））
  - **F_SETFL**：设置文件标志（影响套接字描述符的两个标志：O_NONBLOCK（非阻塞式I/O）、O_ASYNC（信号驱动式I/O））
  -  **F_SETOWN**：指定用于接收SIGIO和SIGURG信号的套接字属主（进程ID或进程组ID） ，其中SIGIO信号是套接字被设置为信号驱动式I/O后产生的，SIGURG信号是在新的带外数据到达套接字时产生的 
    - SIGIO和SIGURG与其他信号的不同之处在于：这两个信号仅在已使用F_SETOWN命令给相关套接字指派了属主后才会产生 
    - F_SETOWN命令的整数类型arg参数既可以是一个正整数，指出接收信号的进程ID，也可以是一个负整数，其绝对值指出接收信号的进程组ID
    - 指定接收信号的套接字属主为一个进程或一个进程组的差别在于：前者仅导致单个进程接收信号，而后者则导致整个进程组中的所有进程接收信号 
    - 使用socket函数新创建的套接字并没有属主。但如果一个新的套接字时从一个监听套接字创建来的，那么该套接字属主将由已连接套接字从监听套接字继承而来
  - **F_GETOWN**：返回套接字的当前属主 
    - F_GETOWN命令把套接字属主作为fcntl函数的返回值返回，它既可以是进程ID，也可以是进程组ID(一个除-1以外的负值) 

  **设置某个文件状态标志唯一正确的方法**：先取得当前标记，与新标记**逻辑或**后再设置标志

  **使用fcntl开启关闭非阻塞式I/O的典型代码**：

  ```c
  int flags;
  
  //开启套接字非阻塞I/O
  //1.先取得当前标记
  if((flags = fcntl(fd,F_GETFL,0)) < 0)
      err_sys("F_GETFL error");
  //2.与新标记逻辑或
  flags |= O_NONBLOCK;
  //3.再设置标志
  if(fcntl(fd,F_SETFL,flags) < 0)
      err_sys("F_SETFL error");
  
  //关闭套接字非阻塞I/O
  flag &= ~O_NONBLOCK;
  if(fcntl(fd,F_SETFL,flags) < 0)
      err_sys("F_GETFL error");
  ```

  

