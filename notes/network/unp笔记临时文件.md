# 第26章 线程

## 26.1 概述

**进程的缺点**：

- 1.fork是昂贵的。fork要把父进程的内存映像复制到子进程，并在子进程中复制所有描述符。现在的fork使用**写时复制**，但仍然昂贵
- 2.fork返回父子进程之间信息的传递需要进程间通信（IPC）机制

线程有助于解决以上问题

**线程的优点**：

- 1.线程的创建可能比进程的创建快10~100倍
- 2.同一进程内的所有线程共享相同的全局内存，这使得线程之间易于共享信息

**线程的问题**：同步问题

**同一进程内所有线程共享的**：

- 1.全局变量
- 2.进程指令
- 3.大多数数据
- 4.打开的文件（即描述符）
- 5.信号处理函数和信号处置
- 6.当前工作目录
- 7.用户ID和组ID

**线程独占的**：

- 1.线程ID
- 2.寄存器集合，包括**程序计数器**和**栈指针**
- 3.栈（用于存放局部变量和返回地址）
- 4.errno
- 5.信号掩码
- 6.优先级

本章讲解的是POSIX线程，也称为Pthread。所有Pthread函数都以`pthread_`打头

## 26.2 基本线程函数：创建和终止

**5个基本线程函数**：

- 1.pthread_create函数：创建主线程以外的其余线程
- 2.pthread_join函数：等待一个给定线程终止
- 3.pthread_self函数：返回线程ID
- 4.pthread_detach函数：把指定线程转变为脱离状态
- 5.pthread_exit函数：终止线程

### 26.2.1 pthread_create函数

当一个程序由exec启动执行时，称为**初始线程**或**主线程**的单个线程就创建了

**pthread_create函数作用**：创建主线程以外的其余线程，类似于进程的fork

```c
#include <pthread.h>

int pthread_create(pthread_t *tid, const pthread_attr_t *attr, void* (*fun)(void*), void* arg);
```

- tid：线程ID，其数据类型为pthread_t（往往是unsigned int），如果新的线程成功创建，其ID就通过tid指针返回
- attr：线程属性，如优先级、初始栈大小、是否应该成为一个守护线程等。创建线程时可以通过初始化一个取代默认设置的pthread_attr_t变量指定这些属性。如果使用默认设置，则attr设为空指针
- fun和arg：线程执行的函数及其参数。该线程通过调用这个函数开始，然后显示终止（调用pthread_exit）或隐式终止（函数返回）。该函数的唯一调用参数是指针arg，需要给该函数传递多个参数，则需要将它们打包成一个结构，然后把这个结构的地址作为单个参数传递给这个起始函数。该函数的返回值是相应线程的终止状态
- 返回值：成功时返回0，若出错则为正的Exxx值。例如：pthread_create因线程数目上超过某个系统限制而不能创建新线程，函数返回值是EAGAIN，Pthread函数不设置errno

### 26.6.2 pthread_join函数

**pthread_join函数作用**：通过调用pthread_join等待一个给定线程终止，类似于进程的waitpid

```c
#include <pthread.h>

int pthread_join(pthread_t *tid, void** status);
```

- tid：等待线程的tid，必须指定，而不能像进程一样等待任意一个线程（类似指定进程ID参数为-1调用waitpid）
- status：如果该指针非空，来自所等待线程的返回值（一个指向某个对象的指针）将存入由status指向的位置
- 返回值：若成功为0，出错为正的Exxx值

### 26.2.3 pthread_self函数

**pthread_self函数作用**：返回线程ID，类似于进程的getpid

```c
#include <pthread.h>

pthread_t pthread_selft(void);
//返回值：调用线程的线程ID
```

### 26.2.4 pthread_detach函数

**背景知识**：一个线程可以是**可汇合的**（joinable，默认值），也可以是**脱离的**（detached）。当一个可汇合的线程终止时，它的线程ID和退出状态将留存到另一个线程对它调用pthread_join。脱离的线程却像守护进程，当它终止时，所有相关资源都被释放，我们不能等待它们终止。

如果一个线程需要知道另一个线程什么时候终止，就最好保持第二个线程的可汇合状态

**pthread_detach函数作用**：把指定的线程转变为脱离状态

```c
#include <pthread.h>

int pthread_detach(pthread_t tid);
//返回值：若成功则为0，若出错则为正的Exxx值
```

本函数通常由想让自己脱离的线程调用：

```c
pthread_detach(pthread_self());
```

### 26.2.5 pthread_exit函数

**pthread_exit函数作用**：让一个线程终止的方法之一

```c
#include <pthread.h>

void pthread_exit(void *status);
//不返回到调用者
```

- status：指针status不能指向局部于调用线程的对象，因为线程终止时这样的对象也小时

如果本线程未曾脱离，它的线程ID和退出状态将一直留存到调用进程内的某个其他线程对它调用pthread_join

**让一个线程终止的其他两个方法**：

- 1.启动线程的函数（即pthread_create的第三个参数fun）可以返回。既然该函数（fun）必须声明成返回一个void指针，它的返回值就是相应线程的终止状态
- 2.如果进程的main函数返回或者任何线程调用了exit，整个进程就终止，其中包括它的任何线程

## 26.3 使用线程的str_cli函数（客户）

![](../../pics/network/unp笔记/Pic_26_1_使用线程重新编写str_cli.png)

```c
// 源码： threads/strclithread.c

#include	"unpthread.h"
//该头文件包含unp.h和pthread.h，然后定义了包裹pthread_打头的一系列函数

void	*copyto(void *);

//创建的线程需要的两个参数
//连接到服务器的TCP套接字描述符
static int	sockfd;		/* global for both threads to access */
//输入文件的标准I/O库FILE指针
static FILE	*fp;

void
str_cli(FILE *fp_arg, int sockfd_arg)
{
	char		recvline[MAXLINE];
	pthread_t	tid;

	sockfd = sockfd_arg;	/* copy arguments to externals */
	fp = fp_arg;

	//创建线程，新线程ID返回到tid，
	//由新线程执行的函数是copyto，没有参数传递给该线程
	Pthread_create(&tid, NULL, copyto, NULL);

	//主线程调用readline读入套接字数据
	while (Readline(sockfd, recvline, MAXLINE) > 0)
		//主线程将套接字读入数据写入标准输出
		Fputs(recvline, stdout);

	//str_cli函数返回时，回到客户main函数，
	//main函数通过调用exit终止进程
	//进程内的所有线程也就随之终止
}

void *
copyto(void *arg)
{
	char	sendline[MAXLINE];

	//如果该线程从标准输入读到EOF，Fgets返回NULL
	while (Fgets(sendline, MAXLINE, fp) != NULL)
		//将从标准输入读到的输入，写入套接字
		Writen(sockfd, sendline, strlen(sendline));

	//如果该线程从标准输入读到EOF，则向服务器发送FIN
	Shutdown(sockfd, SHUT_WR);	/* EOF on stdin, send FIN */

	//如果该线程从标准输入读到EOF，该线程在main函数的exit调用终止之前结束
	//如果服务器过早终止，则尚未读入EOF的该线程将由main函数调用exit终止
	return(NULL);
		/* 4return (i.e., thread terminates) when EOF on stdin */
}
```

**新线程需要的参数传递方法**：

- 1.如代码中所示，使用外部变量
- 2.把这两个值放到一个结构中，然后把指针指向这个结构的一个指针作为参数传递给我们要创建的线程

**性能分析**：该版本略快于使用fork的版本，不过慢于非阻塞式I/O的版本。但非阻塞式I/O版本的复杂度和线程版本的简单性，我们依然推荐使用线程而不是非阻塞式I/O

## 26.4 使用线程的TCP回射服务器程序（main）

该版本的TCP回射服务器程序为每个客户使用一个线程，而不是为每个客户使用一个劲

```c
// 源码： threads/tcpserv01.c

#include	"unpthread.h"

static void	*doit(void *);		/* each thread executes this function */

int
main(int argc, char **argv)
{
	int				listenfd, connfd;
	pthread_t		tid;
	socklen_t		addrlen, len;
	struct sockaddr	*cliaddr;

	if (argc == 2)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 3)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: tcpserv01 [ <host> ] <service or port>");

	cliaddr = Malloc(addrlen);

	for ( ; ; ) {
		len = addrlen;
		connfd = Accept(listenfd, cliaddr, &len);
		//传递给doit函数的唯一参数是已连接套接字描述符
		Pthread_create(&tid, NULL, &doit, (void *) connfd);
		//主线程不关闭已连接套接字，而在fork情形下，服务器程序中是要关闭的。
		//因为同一进程内所有线程共享全部描述符，要是主线程调用close，
		//它就会终止相应的连接。创建新线程并不影响已打开描述符的引用计数
	}
}

static void *
doit(void *arg)
{
	//让自身脱离，因为主线程没有理由等待它创建的每个线程
	Pthread_detach(pthread_self());

	str_echo((int) arg);	/* same function as before */
	//线程必须关闭已连接套接字，因为本线程和主线程共享所有的描述符
	//对于fork的情形，子进程就不必close已连接套接字，
	//因为子进程终止时，会把所有打开的描述符在进程终止时都将被关闭
	Close((int) arg);		/* done with connected socket */
	return(NULL);
}
```

**注意**：把connfd传递给doit时，把connfd类型做强制转换成void指针并不保证所有系统上都起作用

### 26.4.1 给新线程传递参数

把connfd传递给doit时，把connfd类型做强制转换成void指针并不保证所有系统上都起作用。要正确处理这一点需要做额外的工作：不能简单地把connfd的地址传递给新线程

从ANSI C角度看这个是可接受的：ANSI C保证我们能够把一个整数指针类型强制转换成`void*`，然后把这个`void*`指针类型强制转换回原来的整数指针。**问题**出现在这个整数指针指向什么上

主线程中只有一个整数变量connfd，每次调用accept该变量都会被覆写一个新值（已连接描述符）。问题出现在多个线程不是同步地访问一个共享变量（以取得存放在connfd中的整数值）。**解决方法**：

- 1.把该值的一个副本推入被调用函数的栈中
- 2.使用动态内存（如下代码所示）

```c
// 源码： threads/tcpserv02.c

#include	"unpthread.h"

static void	*doit(void *);		/* each thread executes this function */

int
main(int argc, char **argv)
{
	int				listenfd, *iptr;
	thread_t		tid;
	socklen_t		addrlen, len;
	struct sockaddr	*cliaddr;

	if (argc == 2)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 3)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: tcpserv01 [ <host> ] <service or port>");

	cliaddr = Malloc(addrlen);

	for ( ; ; ) {
		len = addrlen;
		//每次调用accpet时，首先调用malloc分配一个整数变量的内存空间，
		//用于存放有待accept返回的已连接描述符，
		//这使得每个线程都有各自的已连接描述符副本
		iptr = Malloc(sizeof(int));
		*iptr = Accept(listenfd, cliaddr, &len);
		Pthread_create(&tid, NULL, &doit, iptr);
	}
}

static void *
doit(void *arg)
{
	int		connfd;

	connfd = *((int *) arg);
	//线程获取已连接描述符的值后调用free释放空间
	free(arg);

	Pthread_detach(pthread_self());
	str_echo(connfd);		/* same function as before */
	Close(connfd);			/* done with connected socket */
	return(NULL);
}
```

**背景知识**：

- 1.**重入**一般可以理解为一个函数在同时多次调用
- 2.在多任务系统当中，在任务执行期间捕捉到信号并对其进行处理时，进程正在执行的指令序列就被信号处理程序临时中断。如果从信号处理程序返回，则继续执行进程断点处的正常指令序列，从重新恢复到断点重新执行的过程中，函数所依赖的环境没有发生改变，就说这个函数是**可重入**的，反之就是**不可重入**的

**问题**：malloc和free两个函数是**不可重入**的，即主线程正处于这两个函数之一的内部处理期间，从某个信号处理函数中调用者两个函数之一有可能导致灾难性的后果，这是因为两个函数操纵相同的静态数据结构

**解决**：POSIX要求这两个函数以及许多其他函数都是**线程安全**的，通常通过在对我们透明的库函数内部执行某种形式的同步达到

### 26.4.2 线程安全函数

除了下图中列出的函数外，POSIX1要求由POSIX.1和ANSC C标准定义的所有函数都是线程安全的（最后5行来源于Unix98）

![](../../pics/network/unp笔记/Pic_26_5_线程安全函数.png)

让一个函数线程安全的共通技巧是定义一个名字以_r结尾的新函数。其中两个函数（ctermid和tmpnam）的线程安全条件是：调用者为返回这预先分配空间，并把指向该空间的指针作为参数传递给函数

## 26.5 线程特定数据



