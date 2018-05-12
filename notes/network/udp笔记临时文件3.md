# 第16章 非阻塞式I/O

套接字的默认状态是**阻塞**的，当发出一个不能立即完成的套接字调用时，其进程被投入睡觉，等待相应操作完成

**非阻塞式IO**：

![](../../pics/network/unp笔记/Pic_6_2_非阻塞式IO模型.png)

可能阻塞的**套接字调用**可分为以下4类： 

- **1.输入操作**：包括read、readv、recv、recvfrom和recvmsg 
  - **阻塞**：如果套接字的接收缓冲区中没有数据可读，进程将被投入睡眠，直到一些数据到达
    - TCP：只要有数据到达（单个字节或万兆的TCP分节），就唤醒进程。如果想要等到某个固定数目的数据为止，使用readn函数或指定MSG_WAITALL标志
    - UDP：只要有UDP数据报到达，就唤醒进程
  - **非阻塞**：如果输入操作不能被满足（TCP至少一个字节，UDP至少一个数据报），相应调用将立即返回一个EWOULDBLOCK错误 
- **2.输出操作**：包括write、writev、send、sendto和sendmsg 
  - **阻塞**： 如果套接字的发送缓冲区中没有空间，进程将被投入睡眠 （**仅对于TCP**）
    - **UDP**：输出函数调用将不会因**空间问题**阻塞，不过有可能因其他原因阻塞
  - **非阻塞**：如果发送缓存区中根本没空间，返回一个EWOULDBLOCK错误；如果有一些空间，返回值是内核能够复制到该缓冲区中的字节数，也称为”不足计数“ （**仅对于TCP**）
- **3.接受连接**：即accept函数
  - **阻塞**：如果尚无新的连接到达，调用进程将被投入睡眠 
  - **非阻塞**：如果尚无新的连接到达，调用将立即返回一个EWOULDBLOCK错误
- **4.发起连接**：用于TCP的connect函数（UDP的connect不是真正的连接） 
  - **阻塞**：connect将阻塞到3路握手的前2路完成（即至少阻塞1个RTT） 
  - **非阻塞**：如果对于一个非阻塞的TCP套接字调用connect，并且连接不能立即建立，那么照样会发起连接（发出3路握手的第1个分组），但会返回一个EINPROGRESS错误
    - 可以立即建立的连接：服务器和客户处于同一个主机 

## 16.2 非阻塞读和写：str_cli函数（非阻塞式I/O select）

非阻塞式I/O的加入让本函数的缓冲区管理显著地复杂化了

本例中需要维护两个缓冲区：

**缓冲区1 to缓冲区**：容纳从标准输入到套接字的数据的缓冲区

- **to**：缓冲区1的起始地址
- **tooptr**：指向下一个必须写到套接字的字节
- **toiptr**：指向从标准输入读入的数据可以存放的下一个字节
- **&to[MAXLINE]**：缓冲区1的终止地址

![](G:\OneDrive\Github\Interview-Summary\pics\network\unp笔记\Pic_16_1_容纳从标准输入到套接字的数据的缓冲区.png)

**缓冲区2 fr缓冲区**：容纳从套接字到标准输出的数据的缓冲区

- **fr**：缓冲区2的起始地址
- **froptr**：指向下一个写到标准输出的数据
- **friptr**：指向从套接字读入的数据可以存放的下一个字节
- **&fr[MAXLINE]**：缓冲区2的终止地址

![](G:\OneDrive\Github\Interview-Summary\pics\network\unp笔记\Pic_16_2_容纳从套接字到标准输出的数据的缓冲区.png)

**str_cli函数**：

```c
// 源码： nonblock/strclinonb.c

/* include nonb1 */
#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	int			maxfdp1, val, stdineof;
	ssize_t		n, nwritten;
	fd_set		rset, wset;
	char		to[MAXLINE], fr[MAXLINE];
	char		*toiptr, *tooptr, *friptr, *froptr;

	//把套接字设为非阻塞
	val = Fcntl(sockfd, F_GETFL, 0);
	Fcntl(sockfd, F_SETFL, val | O_NONBLOCK);

	//把标准输入设为非阻塞
	val = Fcntl(STDIN_FILENO, F_GETFL, 0);
	Fcntl(STDIN_FILENO, F_SETFL, val | O_NONBLOCK);

	//把标准输出设为非阻塞
	val = Fcntl(STDOUT_FILENO, F_GETFL, 0);
	Fcntl(STDOUT_FILENO, F_SETFL, val | O_NONBLOCK);

	//初始化缓冲区指针
	toiptr = tooptr = to;	/* initialize buffer pointers */
	friptr = froptr = fr;
	stdineof = 0;

	maxfdp1 = max(max(STDIN_FILENO, STDOUT_FILENO), sockfd) + 1;
	for ( ; ; ) {
		//初始化读描述符集
		FD_ZERO(&rset);
		//初始化写描述符集
		FD_ZERO(&wset);
		//stdineof == 0 表示标准输入尚未读到EOF
		//toiptr < &to[MAXLINE] 表示还有容纳从标准输入读入的缓冲空间
		//打开读描述符集中对应标准输入的位
		if (stdineof == 0 && toiptr < &to[MAXLINE])
			FD_SET(STDIN_FILENO, &rset);	/* read from stdin */
		//如果还有容纳从套接字读入的缓冲空间
		//打开读描述符集中对应套接字的位
		if (friptr < &fr[MAXLINE])
			FD_SET(sockfd, &rset);			/* read from socket */
		//如果还有要写到套接字的数据
		//打开写描述符集中套接字对应的位
		if (tooptr != toiptr)
			FD_SET(sockfd, &wset);			/* data to write to socket */
		//如果还有要写入到标准输出的数据
		//打开写描述符集中标准输出对应的位
		if (froptr != friptr)
			FD_SET(STDOUT_FILENO, &wset);	/* data to write to stdout */

		Select(maxfdp1, &rset, &wset, NULL, NULL);
/* end nonb1 */
/* include nonb2 */
		if (FD_ISSET(STDIN_FILENO, &rset)) {
			//如果标准输入读就绪，将数据写入to缓冲区中
			if ( (n = read(STDIN_FILENO, toiptr, &to[MAXLINE] - toiptr)) < 0) {
				//如果发生EWOULDBLOCK错误，则忽略它（这里没有处理该错误的代码）
				//通常情况下这种条件“不应该发生”，因为这意味着，select告知我们
				//相应描述符可读，然而read该描述符却返回EWOULDBLOCK错误

				//如果错误不是EWOULDBLOCK错误，则输出该错误
				if (errno != EWOULDBLOCK)
					err_sys("read error on stdin");

			} else if (n == 0) {
#ifdef	VOL2
				fprintf(stderr, "%s: EOF on stdin\n", gf_time());
#endif			//如果read返回0，那么标准输入处理就此结束，设置stdineof标志位1
				stdineof = 1;			/* all done with stdin */
				//如果在to缓冲区中不再有数据要发送，就调用shutdown发送FIN到服务器
				//如果还有数据要发送，FIN的发送就得推迟到缓冲区中数据已写到套接字之后
				if (tooptr == toiptr)
					Shutdown(sockfd, SHUT_WR);/* send FIN */

			} else {
#ifdef	VOL2
				fprintf(stderr, "%s: read %d bytes from stdin\n", gf_time(), n);
#endif			//正常情况
				//当read返回数据时，我们增加toiptr
				//打开写描述符集中与套接字对应的位
				//当套接字写就绪时，可以将数据写到套接字中
				toiptr += n;			/* # just read */
				FD_SET(sockfd, &wset);	/* try and write to socket below */
			}
		}

		if (FD_ISSET(sockfd, &rset)) {
			//如果套接字读就绪，则将输入写入到fr缓冲区中
			if ( (n = read(sockfd, friptr, &fr[MAXLINE] - friptr)) < 0) {
				//如果发生EWOULDBLOCK错误，则忽略它（这里没有处理该错误的代码）
				//通常情况下这种条件“不应该发生”，因为这意味着，select告知我们
				//相应描述符可读，然而read该描述符却返回EWOULDBLOCK错误

				//如果错误不是EWOULDBLOCK错误，则输出该错误
				if (errno != EWOULDBLOCK)
					err_sys("read error on socket");

			} else if (n == 0) {
#ifdef	VOL2
				fprintf(stderr, "%s: EOF on socket\n", gf_time());
#endif			//如果read返回0，表示服务器发送来了FIN（EOF）
				//如果标准输入上已经遇到EOF，则表示没问题，
				//如果标准输入上没有遇到EOF，表示服务器的EOF并非预期，则输出错误
				if (stdineof)
					return;		/* normal termination */
				else
					err_quit("str_cli: server terminated prematurely");

			} else {
#ifdef	VOL2
				fprintf(stderr, "%s: read %d bytes from socket\n",
								gf_time(), n);
#endif			//正常情况
				//从套接字读入一些数据，相应低增加friptr
				//打开写描述符集中与标准输出对应的位
				//如果标准输出写就绪，可以将数据写到标准输出
				friptr += n;		/* # just read */
				FD_SET(STDOUT_FILENO, &wset);	/* try and write below */
			}
		}
/* end nonb2 */
/* include nonb3 */
		//如果标准输出可写，且要写的字节数大于0，则调用write
		if (FD_ISSET(STDOUT_FILENO, &wset) && ( (n = friptr - froptr) > 0)) {
			if ( (nwritten = write(STDOUT_FILENO, froptr, n)) < 0) {
				//如果发生EWOULDBLOCK错误，则忽略它（这里没有处理该错误的代码）
				//在该条件下，完全有可能发生，因为上述代码“处理套接字可读”时，
				//在不清楚write是否会成功的前提下就打开了写描述符集中与标准输出对应的位

				//如果错误不是EWOULDBLOCK错误，则输出该错误
				if (errno != EWOULDBLOCK)
					err_sys("write error to stdout");

			} else {
#ifdef	VOL2
				fprintf(stderr, "%s: wrote %d bytes to stdout\n",
								gf_time(), nwritten);
#endif			
				//写成功，froptr增加已写出的字节数，
				//如果froptr追上friptr，就同时恢复为指向缓冲区开始处
				froptr += nwritten;		/* # just written */
				if (froptr == friptr)
					froptr = friptr = fr;	/* back to beginning of buffer */
			}
		}

		//如果套接字可写，且要写的字节数大于0，则调用write
		if (FD_ISSET(sockfd, &wset) && ( (n = toiptr - tooptr) > 0)) {
			if ( (nwritten = write(sockfd, tooptr, n)) < 0) {
				//如果发生EWOULDBLOCK错误，则忽略它（这里没有处理该错误的代码）
				//在该条件下，完全有可能发生，因为上述代码“处理标准输入可读”时，
				//在不清楚write是否会成功的前提下就打开了写描述符集中与套接字对应的位

				//如果错误不是EWOULDBLOCK错误，则输出该错误
				if (errno != EWOULDBLOCK)
					err_sys("write error to socket");

			} else {
#ifdef	VOL2
				fprintf(stderr, "%s: wrote %d bytes to socket\n",
								gf_time(), nwritten);
#endif			
				//写成功，tooptr增加已写出的字节数
				//如果tooptr追上toiptr，就同时恢复为指向缓冲区开始处
				//且如果标准输入已经EOF，此时需要写到套接字的数据已经写完，
				//则向服务器发送FIN
				tooptr += nwritten;	/* # just written */
				if (tooptr == toiptr) {
					toiptr = tooptr = to;	/* back to beginning of buffer */
					if (stdineof)
						Shutdown(sockfd, SHUT_WR);	/* send FIN */
				}
			}
		}
	}
}
/* end nonb3 */
```

### 16.2.1 str_cli的较简单版本（阻塞式I/O多进程）

每当我们发现需要使用**非阻塞式I/O**时，更简单的方法通常是把应用程序任务划分到多个进程或线程，即使**阻塞式I/O多进程（多线程）**实现

子进程把来自服务器的文本行复制到标准输出，父进程来自标准输入的文本行复制到服务器

![](G:\OneDrive\Github\Interview-Summary\pics\network\unp笔记\Pic_16_9_使用两个进程的str_cli函数.png)

父子进程共享一个套接字：父进程往套接字中写，子进程从套接字中毒。尽管套接字只有一个，其接收缓冲区和发送缓冲区分别只有一个，然而这个套接字却有两个描述符在引用它：一个在父进程中，另一个在子进程中

```c
// 源码： nonblock/strclifork.c

#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	pid_t	pid;
	char	sendline[MAXLINE], recvline[MAXLINE];

	if ( (pid = Fork()) == 0) {		/* child: server -> stdout */
		while (Readline(sockfd, recvline, MAXLINE) > 0)
			Fputs(recvline, stdout);

		//子进程在套接字上遇到EOF（适时的或过早的）
		//如果过早遇到EOF，子进程向服务器发送一个SIGTERM，
		//告诉父进程停止从标准输入到套接字复制数据
		kill(getppid(), SIGTERM);	/* in case parent still running */
		exit(0);
	}

		/* parent: stdin -> server */
	while (Fgets(sendline, MAXLINE, fp) != NULL)
		Writen(sockfd, sendline, strlen(sendline));

	//标准输入遇到EOF
	//父进程往服务器发送FIN（不能调用close）
	//由于子进程需要继续从服务器到标准输出执行数据复制，
	//直到套接字上遇到EOF
	//因此服务器调用pause让自己进入睡眠状态，等到子进程
	//直到捕获一个信号（子进程来的SIGTERM），尽管父进程不主动捕获，
	//但SIGTERM信号的默认行为是终止进程
	Shutdown(sockfd, SHUT_WR);	/* EOF on stdin, send FIN */
	pause();
	return;
}
```

### 16.2.2 str_cli执行时间（所有版本对比）

- 354.0秒，停等版本（第5章）
- 12.3秒，select 阻塞式I/O版本（第6章）
- 6.9秒，非阻塞式I/O版本（第16章）
- 8.7秒，阻塞式I/O多进程版（第16.2.1节）
- 8.5秒，阻塞式I/O多线程版本（第26章）

**总结**：非阻塞式版本最快。阻塞式I/O多进程版稍慢，但比非阻塞式I/O版本代码简单很多，因此更推荐阻塞式I/O多进程版本

## 16.3 非阻塞connect

