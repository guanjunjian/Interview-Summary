# unp笔记

<!-- GFM-TOC -->
* [第2章 传输层：TCP、UDP和SCTP](#第2章-传输层tcpudp和sctp)
    * [2.6 TCP连接的建立和终止](#26-tcp连接的建立和终止)
        * [2.6.1 三路握手](#261-三路握手)
        * [2.6.2 TCP选项](#262-tcp选项)
        * [2.6.3 TCP连接终止](#263-tcp连接终止)
        * [2.6.5 观察分组](#265-观察分组)
    * [2.7 TIME_WAIT状态](#27-time_wait状态)
    * [2.9 端口号](#29-端口号)
    * [2.10 TCP端口号与并发服务器](#210-tcp端口号与并发服务器)
    * [2.11 缓冲区大小及限制](#211-缓冲区大小及限制)
        * [2.11.1 TCP输出](#2111-tcp输出)
        * [2.11.2 UDP输出](#2112-udp输出)
* [第3章 套接字编程简介](#第3章-套接字编程简介)
    * [3.2 套接字地址结构](#32-套接字地址结构)
        * [3.2.1 IPv4套接字地址结构](#321-ipv4套接字地址结构)
        * [3.2.2 通用套接字地址结构](#322-通用套接字地址结构)
        * [3.2.3 IPv6套接字地址结构](#323-ipv6套接字地址结构)
        * [3.2.4 新的通用套接字地址结构](#324-新的通用套接字地址结构)
        * [3.2.5 套接字地址结构的比较](#325-套接字地址结构的比较)
    * [3.3 值-结果参数](#33-值-结果参数)
    * [3.4 字节排序函数](#34-字节排序函数)
    * [3.5 字节操作函数](#35-字节操作函数)
    * [3.6 inet_aton、inet_addr、和inet_ntoa函数](#36-inet_atoninet_addr和inet_ntoa函数)
    * [3.7 inet_pton和inet_ntop函数](#37-inet_pton和inet_ntop函数)
    * [3.8 sock_ntop和相关函数（本书自定义函数）](#38-sock_ntop和相关函数本书自定义函数)
    * [3.9 readn、writen和readline函数（本书自定义函数）](#39-readnwriten和readline函数本书自定义函数)
* [第4章 基本TCP套接字编程](#第4章-基本tcp套接字编程)
    * [4.2 socket函数](#42-socket函数)
    * [4.3 connect函数](#43-connect函数)
    * [4.4 bind函数](#44-bind函数)
    * [4.5 listen函数](#45-listen函数)
    * [4.6 accept函数](#46-accept函数)
    * [4.7 fork和exec函数](#47-fork和exec函数)
    * [4.8 并发服务器](#48-并发服务器)
    * [4.9 close函数](#49-close函数)
    * [4.10 getsockname和getpeername函数](#410-getsockname和getpeername函数)
* [第5章 TCP客户/服务器程序示例](#第5章-tcp客户服务器程序示例)
    * [5.1 概述](#51-概述)
    * [5.2 TCP回射服务器程序：main函数](#52-tcp回射服务器程序main函数)
    * [5.3 TCP回射服务器程序：str_echo函数](#53-tcp回射服务器程序str_echo函数)
    * [5.4 TCP回射客户程序：main函数](#54-tcp回射客户程序main函数)
    * [5.5 TCP回射客户程序：str_cli函数](#55-tcp回射客户程序str_cli函数)
    * [5.6 正常启动](#56-正常启动)
    * [5.7 正常终止](#57-正常终止)
    * [5.8 POSIX信号处理](#58-posix信号处理)
        * [信号基本知识](#信号基本知识)
        * [signal函数](#signal函数)
        * [POSIX信号语义](#posix信号语义)
    * [5.9 处理SIGCHLD信号](#59-处理sigchld信号)
        * [处理僵死进程](#处理僵死进程)
        * [处理被中断的系统调用](#处理被中断的系统调用)
    * [5.10 wait和waipid函数](#510-wait和waipid函数)
        * [函数wait和waipid的区别](#函数wait和waipid的区别)
    * [5.11 accept返回前连接中断](#511-accept返回前连接中断)
    * [5.12 服务器进程终止](#512-服务器进程终止)
    * [5.13 SIGPIPE信号](#513-sigpipe信号)
    * [5.14 服务器主机崩溃](#514-服务器主机崩溃)
    * [5.15 服务器主机崩溃后重启](#515-服务器主机崩溃后重启)
    * [5.16 服务器主机关机](#516-服务器主机关机)
    * [5.18 数据格式](#518-数据格式)
        * [5.18.1 例子：在客户与服务器之间传递文本串](#5181-例子在客户与服务器之间传递文本串)
        * [5.18.2 例子：在客户与服务器之间传递二进制结构](#5182-例子在客户与服务器之间传递二进制结构)
* [第6章 I/0复用：select和poll函数](#第6章-i0复用select和poll函数)
    * [6.2 I/O模型](#62-io模型)
        * [6.2.1 阻塞式I/O模型](#621-阻塞式io模型)
        * [6.2.2 非阻塞式I/O模型](#622-非阻塞式io模型)
        * [6.2.3 I/O复用模型](#623-io复用模型)
        * [6.2.4 信号驱动式I/O模型](#624-信号驱动式io模型)
        * [6.2.5 异步I/O模型](#625-异步io模型)
        * [6.2.6 各种I/O模型的比较](#626-各种io模型的比较)
    * [6.3 select函数](#63-select函数)
        * [6.3.1 描述符就绪条件](#631-描述符就绪条件)
        * [6.3.2 select的最大描述符数](#632-select的最大描述符数)
    * [6.4 str_cli函数（select修订版1）](#64-str_cli函数select修订版1)
    * [6.5 批量输入](#65-批量输入)
    * [6.6 shutdown函数](#66-shutdown函数)
    * [6.7 str_cli函数（select修订版2）](#67-str_cli函数select修订版2)
    * [6.8 TCP回射服务器程序（select修订版）](#68-tcp回射服务器程序select修订版)
    * [6.9 pselect函数](#69-pselect函数)
    * [6.10 poll函数](#610-poll函数)
    * [6.11 TCP回射服务器程序（poll修订版）](#611-tcp回射服务器程序poll修订版)
    * [6.12 epoll](#612-epoll)
* [第7章 套接字选项](#第7章-套接字选项)
    * [7.1 概述](#71-概述)
    * [7.2 getsockopt和setsockopt](#72-getsockopt和setsockopt)
    * [7.4 套接字状态](#74-套接字状态)
    * [7.5 通用套接字选项](#75-通用套接字选项)
        * [7.5.1 SO_BROADCAST套接字选项](#751-so_broadcast套接字选项)
        * [7.5.2 SO_DEBUG套接字选项](#752-so_debug套接字选项)
        * [7.5.3 SO_DONTROUTE套接字选项](#753-so_dontroute套接字选项)
        * [7.5.4 SO_ERROR套接字选项](#754-so_error套接字选项)
        * [7.5.5 SO_KEEPLIVE套接字选项](#755-so_keeplive套接字选项)
        * [7.5.6 SO_LINGER套接字选项](#756-so_linger套接字选项)
        * [7.5.8 SO_RCVBUF和SO_RCVSNDBUF套接字选项](#758-so_rcvbuf和so_rcvsndbuf套接字选项)
        * [7.5.9 SO_RCVLOWAT和SO_SNDLOWAT套接字选项](#759-so_rcvlowat和so_sndlowat套接字选项)
        * [7.5.10 SO_RCVTIMEO和SO_SNDTIMEO套接字选项](#7510-so_rcvtimeo和so_sndtimeo套接字选项)
        * [7.5.11 SO_REUSEADDR和SO_REUSEPOPT套接字](#7511-so_reuseaddr和so_reusepopt套接字)
        * [7.5.12 SO_TYPE套接字选项](#7512-so_type套接字选项)
    * [7.6 IPv4套接字选项](#76-ipv4套接字选项)
        * [7.6.2 IP_OPTIONS套接字选项](#762-ip_options套接字选项)
        * [7.6.3 IP_RECVDSTADDR套接字选项](#763-ip_recvdstaddr套接字选项)
        * [7.6.4 IP_RECVIF套接字选项](#764-ip_recvif套接字选项)
        * [7.6.5 IP_TOS套接字选项](#765-ip_tos套接字选项)
        * [7.6.6 IP_TTL套接字选项](#766-ip_ttl套接字选项)
    * [7.9 TCP套接字选项](#79-tcp套接字选项)
        * [7.9.1 TCP_MAXSEG套接字选项](#791-tcp_maxseg套接字选项)
        * [7.9.2 TCP_NODELAY套接字选项](#792-tcp_nodelay套接字选项)
    * [7.11 fcntl函数](#711-fcntl函数)
* [第8章 基本UDP套接字编程](#第8章-基本udp套接字编程)
    * [8.1 概述](#81-概述)
    * [8.2 recvfrom和sendto函数](#82-recvfrom和sendto函数)
    * [8.3 UDP回射服务器程序：main函数](#83-udp回射服务器程序main函数)
    * [8.4 UDP回射服务器程序：dg_echo函数](#84-udp回射服务器程序dg_echo函数)
    * [8.5 UDP回射客户程序：main函数](#85-udp回射客户程序main函数)
    * [8.6 UDP回射客户程序：dg_cli函数](#86-udp回射客户程序dg_cli函数)
    * [8.7 数据报的丢失](#87-数据报的丢失)
    * [8.8 验证收到的响应（dg_cli服务器IP验证版）](#88-验证收到的响应dg_cli服务器ip验证版)
    * [8.9 服务器进程未运行](#89-服务器进程未运行)
    * [8.10 UDP程序例子小结](#810-udp程序例子小结)
    * [8.11 UDP的connect函数](#811-udp的connect函数)
        * [8.11.1 给一个UDP套接字多次调用connect](#8111-给一个udp套接字多次调用connect)
        * [8.11.2 性能](#8112-性能)
    * [8.12 dg_cli函数（UDP已连接套接字修订版）](#812-dg_cli函数udp已连接套接字修订版)
    * [8.13 UDP缺乏流量控制（dg_echo套接字缓冲区修订版）](#813-udp缺乏流量控制dg_echo套接字缓冲区修订版)
    * [8.14 UDP中的外出接口的确定](#814-udp中的外出接口的确定)
    * [8.15 使用select函数的TCP和UDP回射服务器程序](#815-使用select函数的tcp和udp回射服务器程序)
* [第11章 名字与地址转换](#第11章-名字与地址转换)
    * [11.1 概述](#111-概述)
    * [11.2 域名系统](#112-域名系统)
        * [11.2.1 资源记录](#1121-资源记录)
        * [11.2.2 解析器和名字服务器](#1122-解析器和名字服务器)
        * [11.2.3 DNS替代方法](#1123-dns替代方法)
* [主机名字与IP地址之间的转换](#主机名字与ip地址之间的转换)
    * [11.3 gethostbyname函数](#113-gethostbyname函数)
    * [11.4 gethostbyaddr函数](#114-gethostbyaddr函数)
* [服务名字与端口号之间的转换](#服务名字与端口号之间的转换)
    * [11.5 getservbyname和getservbyport函数](#115-getservbyname和getservbyport函数)
    * [使用gethogetservbyport函数stbyname和getservbyname](#使用gethogetservbyport函数stbyname和getservbyname)
* [主机与服务名字转IP地址与端口号](#主机与服务名字转ip地址与端口号)
    * [11.6 getaddrinfo函数](#116-getaddrinfo函数)
    * [11.7 gai_strerror函数](#117-gai_strerror函数)
    * [11.8 freeaddrinfo函数](#118-freeaddrinfo函数)
    * [11.11 host_serv函数](#1111-host_serv函数)
    * [11.12 tcp_connect函数](#1112-tcp_connect函数)
    * [11.13 tcp_listen函数](#1113-tcp_listen函数)
    * [11.14 udp_client函数](#1114-udp_client函数)
    * [11.15 udp_connect函数](#1115-udp_connect函数)
    * [11.16 udp_server函数](#1116-udp_server函数)
* [IP地址与端口号转主机与服务名字](#ip地址与端口号转主机与服务名字)
    * [11.17 getnameinfo](#1117-getnameinfo)
* [可重入函数](#可重入函数)
    * [11.18 可重入函数](#1118-可重入函数)
    * [11.19 gethostbyname_r和gethostbyaddr_r函数](#1119-gethostbyname_r和gethostbyaddr_r函数)
    * [11.21 其他网络相关信息](#1121-其他网络相关信息)
    * [11.22 小结](#1122-小结)
* [第14章 高级I/O函数](#第14章-高级io函数)
    * [14.2 套接字超时](#142-套接字超时)
        * [14.2.1 使用SIGALRM为connect设置超时](#1421-使用sigalrm为connect设置超时)
        * [14.2.2 使用SIGALRM位recvfrom设置超时](#1422-使用sigalrm位recvfrom设置超时)
        * [14.2.3 使用select为recvfrom设置超时](#1423-使用select为recvfrom设置超时)
        * [14.2.4 使用SO_RCVTIMEO套接字选项为recvfrom设置超时](#1424-使用so_rcvtimeo套接字选项为recvfrom设置超时)
    * [14.7 排队的数据量](#147-排队的数据量)
* [Unix I/O函数](#unix-io函数)
    * [14.3 recv和send函数](#143-recv和send函数)
    * [14.4 readv和writev函数](#144-readv和writev函数)
    * [14.5 14.6 recvmsg和sendmsg、辅助数据](#145-146-recvmsg和sendmsg辅助数据)
        * [recvmsg和sendmsg](#recvmsg和sendmsg)
        * [辅助数据](#辅助数据)
* [标准I/O函数](#标准io函数)
    * [14.8 套接字和标准I/O](#148-套接字和标准io)
* [第15章 Unix域套接字](#第15章-unix域套接字)
    * [15.2 Unix域套接字地址结构](#152-unix域套接字地址结构)
* [Unix域相关函数](#unix域相关函数)
    * [15.3 socketpair函数](#153-socketpair函数)
    * [15.4 套接字函数](#154-套接字函数)
    * [15.5 Unix域字节流客户/服务器程序](#155-unix域字节流客户服务器程序)
    * [15.6 Unix域数据报客户/服务器程序](#156-unix域数据报客户服务器程序)
    * [15.7 描述符传递](#157-描述符传递)
* [第16章 非阻塞式I/O](#第16章-非阻塞式io)
    * [16.2 非阻塞读和写：str_cli函数（非阻塞式I/O select）](#162-非阻塞读和写str_cli函数非阻塞式io-select)
        * [16.2.1 str_cli的较简单版本（阻塞式I/O多进程）](#1621-str_cli的较简单版本阻塞式io多进程)
        * [16.2.2 str_cli执行时间（所有版本对比）](#1622-str_cli执行时间所有版本对比)
    * [16.3 非阻塞connect](#163-非阻塞connect)
    * [16.4 非阻塞connect：时间获取客户程序](#164-非阻塞connect时间获取客户程序)
    * [16.5 非阻塞connect：Web客户程序](#165-非阻塞connectweb客户程序)
    * [16.6 非阻塞accpet](#166-非阻塞accpet)
* [第26章 线程](#第26章-线程)
    * [26.1 概述](#261-概述)
    * [26.2 基本线程函数：创建和终止](#262-基本线程函数创建和终止)
        * [26.2.1 pthread_create函数](#2621-pthread_create函数)
        * [26.6.2 pthread_join函数](#2662-pthread_join函数)
        * [26.2.3 pthread_self函数](#2623-pthread_self函数)
        * [26.2.4 pthread_detach函数](#2624-pthread_detach函数)
        * [26.2.5 pthread_exit函数](#2625-pthread_exit函数)
    * [26.3 使用线程的str_cli函数（客户）](#263-使用线程的str_cli函数客户)
    * [26.4 使用线程的TCP回射服务器程序（main）](#264-使用线程的tcp回射服务器程序main)
        * [26.4.1 给新线程传递参数](#2641-给新线程传递参数)
        * [26.4.2 线程安全函数](#2642-线程安全函数)
    * [26.5 线程特定数据](#265-线程特定数据)
    * [26.6 Web客户与同时连接（Solaris线程）](#266-web客户与同时连接solaris线程)
    * [26.7 互斥锁](#267-互斥锁)
    * [26.8 条件变量](#268-条件变量)
    * [26.9 Web客户与同时连接（Pthread线程）](#269-web客户与同时连接pthread线程)
* [第30章 服务/服务器程序设计范式](#第30章-服务服务器程序设计范式)
    * [30.1 概述](#301-概述)
    * [30.2 TCP客户程序设计范式](#302-tcp客户程序设计范式)
    * [30.3 TCP测试用客户程序](#303-tcp测试用客户程序)
    * [30.4 TCP迭代服务器程序](#304-tcp迭代服务器程序)
    * [30.5 TCP并发服务器程序，每个客户一个子进程](#305-tcp并发服务器程序，每个客户一个子进程)
    * [30.6 TCP预先派生子进程服务器程序，accept无上锁保护](#306-tcp预先派生子进程服务器程序，accept无上锁保护)
        * [30.6.1 4.4BSD上实现多进程accept同一监听描述符](#3061-44bsd上实现多进程accept同一监听描述符)
        * [30.6.3 连接在子进程中的分布](#3063-连接在子进程中的分布)
        * [30.6.4 select冲突](#3064-select冲突)
    * [3.7 TCP预先派生子进程服务器程序，accept使用文件上锁保护](#37-tcp预先派生子进程服务器程序，accept使用文件上锁保护)
    * [30.8 TCP预先派生子进程服务器程序，accept使用线程上锁保护](#308-tcp预先派生子进程服务器程序，accept使用线程上锁保护)
    * [30.9 TCP预先派生子进程服务器程序，传递描述符](#309-tcp预先派生子进程服务器程序，传递描述符)
    * [30.10 TCP并发服务器程序，每个客户一个线程](#3010-tcp并发服务器程序，每个客户一个线程)
    * [30.11 TCP预先创建线程服务器程序，每个线程各自accept](#3011-tcp预先创建线程服务器程序，每个线程各自accept)
    * [30.12 TCP预先创建线程服务器程序，主线程统一accept](#3012-tcp预先创建线程服务器程序，主线程统一accept)
    * [30.13 小结](#3013-小结)
<!-- GFM-TOC -->

# 第2章 传输层：TCP、UDP和SCTP

## 2.6 TCP连接的建立和终止

### 2.6.1 三路握手

- 1.主动打开（通常为客户端）：发送第一个SYN的一端
- 2.被动打开（通常为服务器）：接收第一个SYN的一端
- 3.每一个SYN的ACK的确认号都是该SYN的初始序列号+1，因为SYN占据一个字节的序列号空间。FIN同理

### 2.6.2 TCP选项

![](../../pics/interview/network/TCP首部.png)

这里的内容都是选项字段中的。

- 1.MSS选项：每一端都有
  - 表示：该端在本次连接的每个TCP分节中愿意接受的最大数据量
  - TCP_MAXSEG设置和提取
- 2.窗口规模选项：每一端都有
  - 表示：接收窗口
  - 旧时：因为窗口字段只有16位，因此窗口大小最大为2^16，即65535
  - 新时：在选项字段中，添加14位偏移，因此最大为65535*（2^14），需要两端都支持
  - SO_RCVBUF将影响这个选项
- 3.时间戳选择：
  - 表示：防止由失而复现的分组可能造成的数据损坏
  - 失而复得的分组：不是超时重传的分组。是暂时的路由原因造成的迷途分组，路由稳定后又正常到达目的地

### 2.6.3 TCP连接终止

- 1.主动关闭，首先发送FIN的一端
- 2.被动关闭，首先收到FIN的一端
- 3.被动关闭收到FIN后，将FIN作为文件结束符（EOF）传递给应用程序，放在所有数据之后。表示本端应用程序不会再收到对端的数据
- 4.不一定是4次个分组进行4次挥手：
  - 第一个FIN可能与最后的数据一起发送
  - 第一个FIN的ACK可能与第二个FIN一起发送
- 5.FIN的每个ACK确认号是这个FIN的序列号+1
- 6.半关闭：
  - 表示：被动关闭的一端还可以向主动关闭的一端发送数据
  - 实现：shutdown函数，参数为SHUT_WR（关闭写）
- 7.发送FIN的原因：
  - 1).调用close，并且该套接字的引用计数为0
  - 2).调用shutdown，参数为SHUT_RDWR或SHUT_WR（半关闭）
  - 3).自愿：进程调用exit或main函数退出
  - 4).非自愿：收到一个终止本进程的信号
- 8.任何一端都可以执行主动关闭
  - 通常：客户端
  - 例外：HTTP/1.0，服务器执行主动关闭

### 2.6.5 观察分组

- 第二次握手中的ACK和SYN也是有可能分开发送的
  - 如果服务器产生应答（SYN）的时间少于200ms，则SYN&ACK一起发送
  - 如果服务器产生应答的时间大于1s，则先发送ACK，再发送SYN

## 2.7 TIME_WAIT状态

- 1.MSL可能的时间为30秒~2分钟
- 2.TIME_WAIT可能的时间为1分钟~4分钟
- 3.存在TIME_WAIT的原因
  - 1).可靠地实现TCP全双工连接的终止
    - 如果没有WAIT_TIME。如果最后一个ACK丢失，被动关闭端回重新发送FIN，此时主动关闭端没有WAIT_TIME状态，将这个FIN以RST回应，被动关闭端会将RST解释成一个错误
  - 2).允许老的重复分节在网络中小时
    - 不允许在TIME_WAIT状态，再以同样的IP和端口创建连接
    - 否则，1).中提到的丢失的ACK其实没有丢失，有可能只是迷途了，那么TIME_WAIT的对端就回收到这个ACK而发生错误
- 4.TIME_WAIT是2MSL的原因
  - 每个方向上的分组都需要一个MSL来确保消逝

## 2.9 端口号

- 1.众所周知的端口：0~1023
  - 也是“保留端口”，需要超级用户特权启动
- 2.已经登记的端口：1024~49151
- 3.动态或私用端口：49152~65535
  - 也叫“临时端口”
  - 49152为65535的四分之三
- 4.套接字对：一个定义该连接的两个端点的四元组
  - 本地IP地址、本地TCP端口号
  - 外地IP地址、外地TCP端口号

## 2.10 TCP端口号与并发服务器

![](../../pics/network/unp笔记/Pic_2_14_TCP端口与并发服务器.png)

- 1.多宿主机：多个网卡、多个IP
- 2.服务器可选择一个IP或任意IP作为接受连接的IP地址（不能是指定的多个）
  - 任意IP：使用通配地址，在设置套接字地址结构时，将IP地址字段设为INADDR_ANY
- 3.已连接套接字和监听套接字都使用服务器的21端口
- 4.TCP无法通过查看目的端口号来区分外来节点到不同的端点（进程），因为本地端口21有3个套接字，必须查看4元素才能确定到达哪个端点接收某个到达的分节。例如：
  - 如果分节来自206.168.112.219:1500，则到达子进程1（连接套接字）
  - 如果分节来自206.168.112.219:1501，则到达子进程2（连接套接字）
  - 所有的目的端口为21的其他TCP分节到达父进程（监听套接字）

## 2.11 缓冲区大小及限制

**IPv4**

![](../../pics/network/unp笔记/Pic_A_1_IPv4首部格式.png)

- 1.IPv4最大大小是2^16（65535，包括IPv4首部，实际载荷为65535-20）


- 2.IPV4最小链路MTU为68，因为IPv4首部=20字节+40多字节的选项部分
- 3.路径MTU：两个主机之间的路径中最小的MTU
- 4.分片：这些分片在到达最终目的之前通常不重组
  - IPv4与IPv6的区别：
    - IPv4：主机对其产生和路由对其转发的数据报进行分片
    - IPv6：主机对其产生的数据报分片，路由不对其转发的数据报进行分片（路由对其产生的还是会分片的）
  - DF位：置位表示，不允许分片
    - IPv4：当路由器收到一个超出其外出链路MTU大小且设置了DF位的IPv4数据报时，将产生一个ICMPv4（目的不可达，需分片但DF已置位）
    - IPv6：隐含一个DF位（IPv6本来就不允许路由转发时分片），将产生一个ICMPv6（分组太大）
- 5.DF位的作用：发现路径MTU
- 7.最小重组缓冲区大小，表示IP的任何实现版本都必须保证的最小数据报大小（即双方还没开始交换MSS大小）
  - IPv4：576字节
  - IPv6：1500字节
- 8.MSS：用于向对端TCP通告对端在每个分节中能发送的最大TCP数据量，目的是告诉对端其重组缓冲区大小的实际值，从而避免分片
  - 通常值：MTU-IP首部固定长度-TCP首部固定长度（以太网为：1500-20-20=1460）
  - 实现：通过SYN分节（第一次和第二次握手）上的MSS选项设置
  - 最大值：2^16（65535）字节

### 2.11.1 TCP输出

![](../../pics/network/unp笔记/Pic_2_15_应用进程写TCP套接字时涉及的步骤和缓冲区.png)

这里描述的是应用进程写数据到一个TCP套接字中时发生的步骤。

这里涉及以下知识点：

- 1.每一个TCP套接字都有一个发送缓冲区，由SO_SNDBUF更改大小
- 2.write调用发生的事情：
  - 内核从应用进程的缓冲区中复制所有数据写到套接字的发送缓冲区
  - 如果套接字缓冲区无法容下，可能原因：
    - 应用进程的缓冲区大于套接字的发送缓冲区
    - 套接字的发送缓冲区已有其他数据
  - 如果无法容下，进程被睡眠
  - 内核不从write返回（假设是阻塞的套接字） ，直到应用进程的缓冲区所有的数据都复制到套接字缓冲区中
  - 如果write返回，表示当前进程数据已经复制到套接字缓冲区，进程缓冲区可继续使用。但不表示对端已经收到
- 3.TCP套接字缓冲区需要保留已经发送的数据，直到收到该数据的ACK

### 2.11.2 UDP输出

![](../../pics/network/unp笔记/Pic_2_16_应用进程写UDP套接字时涉及的步骤与缓冲区.png)

这里描述的是应用进程写数据到一个UDP套接字中时发生的步骤。

这里涉及以下知识点：

- 1.UDP有发送缓冲区（通过SO_SNDBUF设置）,实际上不存在（因此上图用虚线），它表示的只是该UDP套接字的数据报上限。
- 2.应用程序写一个大于套接字发送缓冲区大小的数据报，内核返回进程EMSGSIZE错误
- 3.因UDP不可靠，不必保留应用进程的数据副本，因此无需一个真正的发送缓冲区
- 4.数据保存的过程：应用进程的数据在沿协议向下传递时，通常被复制到某种格式的一个内核缓冲区中，当数据发送后，副本被链路层丢弃
- 5.UDP的write成功返回表示所写的数据报或其所有分段已经被加入数据链路层的输出队列
- 6.如果链路层输出队列没有足够的空间存放数据报或分段，返回ENOBUFS错误到进程


# 第3章 套接字编程简介

[UNPv1第三章：套接字编程简介](https://blog.csdn.net/lxj1137800599/article/details/51244629)

## 3.2 套接字地址结构

### 3.2.1 IPv4套接字地址结构

IPv4套接字地址结构通常也称为“网际套接字地址结构”

```c
//<netinet/in.h>
struct in_addr {
    in_addr_t s_addr; //32位IPv4地址网络字节序
    				
};

struct sockaddr_in{
 　　uint8_t             sin_len;          //带符号8位整数地址结构长度，用于简化套接字地址结构的处理
 　　sa_family_t         sin_family;       //协议族，IPv4为AF_INET
 　　in_port_t           sin_port;         //TCP或UDP端口号，16字节，网络字节序
 　　struct in_addr       sin_addr;         //32位IPv4，网络字节序地址
 　　char                sin_zero[8];      //填充对齐位，未使用，通常为0
};
//必须的字段：sin_family、sin_port、sin_addr
//sockaddr_in至少16字节
//总是在填写该结构前将整个结构置为0
//该结构仅在给定主机上使用，不在主机之间传递
```

从进程到内核传递套接字地址结构的函数有4个，它们都要调用sockargs函数

- 1.bind
- 2.connect
- 3.sendto
- 4.sendmsg

从内核到进程传递套接字地址结构的函数有5个

- 1.accept
- 2.recvfrom
- 3.recvmsg
- 4.getpeername
- 5.getsockname

### 3.2.2 通用套接字地址结构

用处：兼容所有协议的套接字地址结构，对指向特定于协议的套接字地址结构的指针执行类型强制转换

ANSI C后，可以使用void*解决

```c
//<sys/socket.h>
struct sockaddr {
  uint8_t            sa_len;
  sa_family_t        sa_family;       /* address family: AF_xxx value */
  char               sa_data[14];     /* protocol-specific address */
};

//例子：
struct sockaddr_in  serv;      /* IPv4 socket address structure */
bind(sockfd, (struct sockaddr *) &serv, sizeof(serv));
```

### 3.2.3 IPv6套接字地址结构

```c
//<netinet/in.h>
struct in6_addr{
 　　uint8_t            s6_addr[16];            //128位IPv6网络字节序地址
};

#define SIN6_LEN                                //编译时测试所需
struct sockaddr_in6{
 　 uint8_t             sin6_len;               //这个结构的长度
 　 sa_family_t         sin6_family;            //协议族，AF_INET6
 　 in_port_t           sin6_port;              //端口号，网络字节序

　　uint32_t            sin6_flowinfo;          //流信息，未定义
 　 struct in6_addr     sin6_addr;              //IPv6地址，网络字节序

　　uint32_t            sin6_scope_id;          //一定范围的接口
 };
```

### 3.2.4 新的通用套接字地址结构

```c
//<netinet/in.h>
struct sockaddr_storage {
  uint8_t      ss_len;       /* length of this struct (implementation dependent) */
  sa_family_t  ss_family;    /* address family: AF_xxx value */
  /*其他字段对用户来说的透明的 故没有列出*/  
};
//优势：足以容纳系统所支持的任何套接字地址结构
```

与sockaddr的对比：

- 1.sockaddr_storage通用套接字地址结构满足对齐要求
- 2.sockaddr_storage通用套接字地址结构足够大，能够容纳系统支持的任何套接字地址结构

### 3.2.5 套接字地址结构的比较

![](../../pics/network/unp笔记/Pic_3_6_不同套接字地址结构的比较.png)

## 3.3 值-结果参数

**1.从进程到内核传递套接字结构函数**

![](../../pics/network/unp笔记/Pic_3_7_从进程到内核传递套接字地址结构.png)

- bind
- connect
- sendto

这些函数的一个参数是指向某个套接字地址结构的指针，另一个参数是该结构体的整数大小 

```c
struct sockaddr_in serv;
connect (sockfd, (SA *) &serv, sizeof(serv));
```

**2.从内核到进程传递套接字地址结构的函数**

![](../../pics/network/unp笔记/Pic_3_8_从内核到进程传递套接字地址结构.png)

- accept
- recvfrom
- getsockname
- getpeername

两个参数指向某个套接字结构体的指针和指向表示该结构体大小的整数变量的指针

```c
struct sockaddr_un cli; /* Unix domain */
socklen_t len;
len = sizeof(cli); /* len is a value */
getpeername(unixfd, (SA *) &cli, &len);	
/* len may have changed */
```

长度为指针的原因（什么是值-结果参数 Value-result）：

- 函数调用时：结构大小len是一个值（value）, 它告诉内核cli结构的大小，使内核在写cli结构时不至于越界
- 函数返回时：结构大小len是一个结果（result），它告诉进程内核在cli结构中确切存储了多少信息

## 3.4 字节排序函数

**小端字节序**：将低序字节存储在起始地址

**大端字节序**：将高序字节存储在起始地址

”大端“、”小端“表示多个字节值的哪一端（小端或大端）存储在该值的起始地址

**MSB**：most significant bit，最高有效位

**LSB**：least significant bit，最低有效位

**主机字节序**：某个给定系统所用的字节序称为主机字节序（不同系统有不同）

![](../../pics/network/unp笔记/Pic_3_9_16位整数的小端字节序和大端字节序.png)

```c
union {
	  short  s;
      char   c[sizeof(short)];
    } un;
un.s = 0x0102;
if(un.c[0] == 1 && un.c[1] == 2); //大端
if (un.c[0] == 2 && un.c[1] == 1); //小端
```

**网络字节序**：网络协议使用**大端字节序**来传递这些字节整数

主机字节序与网络字节序之间的转换函数：

```c
#include <netinet/in.h>
//h表示host, n表示network，s代表short，l代表long
//均返回：网络字节序的值
uint16_t htons(uint16_t host16bitvalue);    //主机to网络 short
uint32_t htonl(uint32_t host32bitvalue);    //主机to网络 long

//均返回：主机字节序的值
uint16_t ntohs(uint16_t net16bitvalue);     //网络to主机 short
uint32_t ntohl(uint32_t net32bitvalue);     //网络to主机 long

```

**字节==八位组==8位的量**

**位序**：按照在线缆上出现的顺序排列的4个字节（32个位）

![](../../pics/network/unp笔记/位序.png)

## 3.5 字节操作函数

**Berkely**

```c
#include <strings.h>
void bzero(void *dest,size_t nbytes);               //将目标字符串制定数目的字节置0
void bcopy(const void *src,void *dest,size_t nbytes);         //拷贝指定字节
int bcmp(const void*ptrl,const void *ptr2,size_t nbytes);     //若相等返回0
```

**ANSI C**

```c
#include<string.h>
void *memset(void *dest,const void *c,size_t len);
void *memcpy(void *dest,const void *src,size_t nbytes);
int memcmp(const void *ptrl,const void *ptr2,size_t nbytes);
//返回：若相等则为0，否则为<0或者>0
```

**注意：**

- 1.memcpy类似bcopy
- 2.bcopy能正确处理源字符串与目的字符串重叠情况
- 3.memcpy不能正确处理源字符串与目的字符串重叠情况，可以使用memmove替代
- 4.memcmp需要将比较的字节转换为无符号字符（unsigned char）

## 3.6 inet_aton、inet_addr、和inet_ntoa函数

地址转换函数

inet_aton、inet_addr和inet_ntoa在点分十进制数串（eg：“206.168.112.96”）与它的32位网络字节序二进制值间转换IPv4地址。

```c
#include<arpa/inet.h>

//作用：将strptr所指的C字符串转换成32位的网络字节序二进制值，并通过指针addrptr来存储
//返回：1--字符串有效， 0--字符串有错
//如果strptr为空，仍然对输入函数进行有效性检查，但addrptr不存储任何结果
int inet_aton(const char *strptr, struct in_addr *addrptr);　

//返回：若成功，返回32位二进制的网络字节序地址；若有错，返回INADDR_NONE
//作用与inet_aton一样，但是返回的是二进制值
//不能处理255.255.255.255,因为它的错误返回值INADDR_NONE也是这个值
//该函数已被废弃，使用inet_aton代替
in_addr_t inet_addr(const char *strptr);

//作用：将一个32位的网络字节序二进制IPv4地址转换成相应的点分十进制数串
//返回：指向点分十进制数串的指针
//该函数不可重入,即不能被中断，因为函数返回值所指的串驻留在静态内存中
//注意：以结构为参数，而不是指向结构的指针
char *inet ntoa(struct in_addr inaddr);
　　　　　　　　　　　             
```

## 3.7 inet_pton和inet_ntop函数

IPv4地址和IPv6地址都可用

函数名中p和n分别代表表达(presentation)和数值(numeric)地址的表达式通常是ASCII字符串

表达式即ASCII字符串

数值即存放在套接字地址结构中的二进制值

```c
#include <arpa/inet.h>

//family可以是AF_INET和AF_INET6

//作用：ASCII字符串--->套接字地址结构中的二进制值
//返回：若成功为1，若输入不是有效的表达式为0，若出错为-1
int inet_pton(int family, const char *strptr, void *addrptr);

//作用：套接字地址结构中的二进制值--->ASCII字符串
//注意：strptr不能为空，调用者必须为目标存储单元分配内存并制定其大小
//返回：若成功则为指向结果的指针，若出错则为null
const char *inet_ntop(int family, const void *addrptr, char *strptr, size_t len);

//len的定义
//<netinet/in.h>
#define INET_ADDRSTRLEN 16
#define INET6_ADDRSTRLEN 46
```

![](../../pics/network/unp笔记/Pic_3_11_地址转换函数小结.png)

## 3.8 sock_ntop和相关函数（本书自定义函数）

本书自定义函数

```c
#include "unp.h"
#define	SA	struct sockaddr

//将统配地址和一个临时端口绑定到一个套接字
int		 sock_bind_wild(int sockfd, int family);
//比较两个套接字地址结构的地址部分
int		 sock_cmp_addr(const SA *sockaddr1, const SA *sockaddr2, socklen_t addrlen);
//比较两个套接字地址结构的端口号部分
int		 sock_cmp_port(const SA *sockaddr1, const SA *sockaddr2, socklen_t addrlen);
//只返回端口号
int		 sock_get_port(const SA *sockaddr, socklen_t addrlen);
//把一个套接字地址结构中的主机部分转换为表达格式（不包括端口号）
char	*sock_ntop_host(const SA *sockaddr, socklen_t addrlen);
//把一个套接字地址结构中的地址部分设为ptr所指的值
void	 sock_set_addr(SA *sockaddr, socklen_t addrlen, const void *ptr);
//只设置一个套接字地址结构的端口号部分
void	 sock_set_port(SA *sockaddr, socklen_t addrlen, int port);
//把一个套接字地址结构中的地址部分置为通配地址
void	 sock_set_wild(SA *sockaddr, socklen_t addrlen);
```

## 3.9 readn、writen和readline函数（本书自定义函数）

字节流套接字上调用read或write输入或输出的字节数可能比请求的数量少

- 原因：套接字的缓冲区可能已经达到了极限
- 解决方法：
  - 1.再次调用read或write函数，以输入或输出剩余的字节
  - 2.实现自定义函数

自定义的目的：实现一个输入或输出的字节数与请求数量一样的函数

```c
#include "unp.h"
//实际上就是read、write函数增强版
ssize_t readn(int filedes, void *buff, size_t nbytes);
ssize_t writen(int filedes, const void *buff, size_t nbytes);
ssize_t readline(int filedes, void *buff, size_t maxlen);
//均返回：读或写的字节数，若出错则为-1，连接已经关闭为0
```

readline原本的实现版本较慢，原因是每次调用read只读取一个字符。可以通过自己实现一个my_read，维护一个自己的缓冲区，readline调用自己实现的my_read从而得到一个较快的版本

# 第4章 基本TCP套接字编程

[UNPv1第四章：基本TCP套接口编程](https://blog.csdn.net/lxj1137800599/article/details/51247813)

连接过程图：

![](../../pics/network/unp笔记/Pic_4_1_基本TCP客户_服务器程序的套接字函数.png)

## 4.2 socket函数

```c
#include <sys/socket.h>
/*
** 用处：创建套接字
** @family:协议族、协议域
** @type:套接字类型
** @protocol:协议类型
** @返回值：成功时返回文件描述符（套接字描述符），错误时返回-1
*/
int socket (int family, int type, int protocol);
```

**family:**

![](../../pics/network/unp笔记/Pic_4_2_socket函数的family常值.png)

**type:**

![](../../pics/network/unp笔记/Pic_4_3_socket函数的type常值.png)

**protocol:**

![](../../pics/network/unp笔记/Pic_4_4_socket函数AF_INET或AF_INET6的protocol常值.png)

**family与type组合是否有效**

![](../../pics/network/unp笔记/Pic_4_5_socket函数中family和type参数的组合.png)

**对比AF_XX和PF_XX**

`AF_`前缀便是地址族，`PF_`前缀表示协议族

曾经的想法：单个协议族可以支持多个多个地址族，`PF_`用于创建套接字，`AF_`用于套接字地址结构

实际上：支持多个地址族的协议族未实现过

头文件`<sys/socket.h>`中，给定协议定义的`PF_`和`AF_`值总是相等的

更多使用`AF_`，本书亦是如此

## 4.3 connect函数

```c
#include <sys/socket.h>
/*
** 用处：建立与TCP服务器的连接
** @sockfd:由socket函数返回的套接字描述符
** @servaddr:指向套接字地址结构的指针
** @addrlen:servaddr结构的长度
** @返回值：成功返回0，失败返回-1
*/
int connect(int sockfd, const struct sockaddr *servaddr, socklen_t addrlen);
```

TCP套接字调用connect函数将触发三次握手过程，在连接成功或失败时返回。

**出错的返回的情况有：**

- 1.超时错误：若TCP客户没有收到SYN包的响应，则返回**ETIMEDOUT**错误
- 2.RST错误：若对客户端的SYN的响应是RST（表示复位），则表明该服务器主机在我们制定的端口没有进程在等待与之连接（例如服务器进程也许没有在允许），返回**ECONNREFUSED**错误，这是一种**硬错误**
- 3.目的地不可达：若客户发出的SYN在中间的某个路由器上引发一个“destination unreachable”（目的地不可达）ICMP错误，多次重传超时后仍然不可达，则保存的ICMP消息作为**EHOSTUNREACH**或**ENETUNREACH**错误返回给进程，这是一种**软错误**

若connect失败则该套接字不再可用，必须关闭，不能对该套接字再次调用connect函数。每次调用connect失败后，必须close当前的套接字描述符并重新调用socket

**RST产生的条件**

- 1.端口未监听：目的地为某个端口的YSN到达，然而该端口没有正在监听的服务器
- 2.取消连接：TCP想取消一个已有连接
- 3.收到无法解释的分节：TCP接收到一个根本不存在的连接上的分节

## 4.4 bind函数

```c
#include <sys/socket.h>
/*
** 作用：把一个本地协议地址赋予一个套接字
** @sockfd：由socket函数返回的套接字描述符
** @myaddr：一个指向特定于协议的地址结构的指针
** @addrlen：myaddr地址结构的长度
** @返回值：成功返回0，失败返回-1
*/
int bind (int sockfd, const struct sockaddr *myaddr, socklen_t addrlen);
```

本地协议地址：由32位的IPv4地址或128位的IPv6地址与16位的TCP或UDP端口号的结合

**常见的bind出错情况：**返回**EADDRINUSE**（“Address already in use”，地址已经使用）

**关于端口绑定：**

如果一个TCP客户或服务器未曾调用bind绑定一个端口，在调用connect或listen时，内核就为相应的套接字选择一个临时端口。

通常，服务器是绑定“众所周知端口”，例外是，远程过程调用服务器，使用内核为其选择的临时端口

如果让内核为套接字选择一个临时端口，为了得到内核所选的这个临时端口值，调用`getsockname`函数来返回协议地址

**关于地址绑定：**

TCP客户端通常不把IP地址绑定到它的套接字上，当连接套接字时，内核将根据所用外出网络接口来选择IP地址

TCP服务器没有把IP地址绑定到它的套接字上，内核就把客户端发送的SYN的目的IP地址作为服务器的源IP地址

**绑定与不绑定**：

bind可以指定IP地址或端口，也可以两者都指定，也可以两者都不指定

![](../../pics/network/unp笔记/Pic_4_6_给bind函数指定要绑定的IP地址和_或端口号产生的结果.png)

**关于通配地址**

对于IPv4，通配地址由常值INADDR_ANY指定，其值一般为0

```c
struct sockaddr_in servaddr;
servaddr.sin_addr.s_addr = htonl( INADDR_ANY );
```

对于IPv6，因为地址是存放在一个结构中的，因此不能直接用INADDR_ANY常量，而应该使用如下方式：

```c
struct sockaddr_in6 serv;
serv.sin6_addr = in6addr_any;
//in6addr_any变量由系统预分配，并初始化为常值IN6ADDR_ANY_INIT
```

## 4.5 listen函数

```c
#include <sys/socket.h>
/*
** 作用：把未连接的套接字变为被动套接字（监听套接字），指示内核应该接受指向该套接字的连接请求。相对的，TCP客户端用于主动发起连接的套接字称为主动套接字
** @sockfd：由socket函数返回的套接字描述符,未连接的套接字
** @backlog:队列长度（未完成队列+已完成队列），但这个值的确切含有对于不同系统有不同的解释
*/
int listen (int sockfd, int backlog);
```

内核为任何一个给定的监听套接字维护**两个队列**：

- 1.未完成队列：已经完成第二次握手（服务器发出SYN&ACK），处于SYN_RCVD的套接字
  - 直到三次握手的三个个分节到达或该套接字超时，则该套接字被移除未完成队列
  - 套接字超时为RTT，因此该队列中的任何一项保留时间为RTT，该值取决于特定的客户端和服务器
- 2.已完成队列：已经完成三次握手，处于ESTABLISHED的套接字，等待服务器进程accept取走这些套接字
  - 当进程调用accept函数时，已完成队列中的队头项将返回给进程，或如果队列为空，那么进程将被投入睡眠，直到TCP在该队列中放入一项才唤醒

![](../../pics/network/unp笔记/Pic_4_7_TCP为监听套接字维护的两个队列.png)

![](../../pics/network/unp笔记/Pic_4_8_TCP三次握手和监听套接字的两个队列.png)



**队列满时的处理**

当一个客户SYN到达时，若未完成队列是满的，TCP忽略该分节，且不会发送RST

不发送的原因：

- 1.这种情况是暂时的，客户端TCP将重发SYN，期待不久就能在这些队列中找到可用空间
- 2.若发送RST，客户端connect会返回错误，客户端就需要处理错误，而不是让TCP重传机制来处理
- 3.客户无法区影响SYN的RST意味着“该端口没有服务器在监听”还是“端口有服务器在监听，但它队列满了”

**三次握手完成之后，accept之前**

在此阶段，服务器收到客户端的数据由服务器TCP排队，最大数量为相应套接字的缓冲区大小

**关于安全性**

- 1.SYN洪泛：给受害者主机发送SYN，用以装填一个或多个TCP端口的未完成连接队列
- 2.IP欺骗：在SYN洪泛中，每个SYN的源IP地址都是随机数，这样服务器的SYN/ACK就不知道发往什么地方

## 4.6 accept函数

```c
#include <sys/socket.h>
/*
** 作用：用于从已完成连接队列列头返回下一个已完成连接，如果已完成连接队列为空，进程将被投入睡眠（假设套接字为默认的阻塞方式）
** @sockfd：调用listen由未连接套接字变为的监听套接字
** @cliaddr:指向客户端的协议地址结构的指针
** @addrlen:是一个指针，是值-结果参数。调用前，cliaddr所指向套接字地址结构的长度；返回时，为由内核放在该套接字地址结构内的确切字节数
** @返回值：若成功返回套接字描述符（与客户端的已连接套接字），出错返回-1
*/
int accept (int sockfd, struct sockaddr *cliaddr, socklen_t *addrlen);
```

**监听套接字与已连接套接字的区别**

- 监听套接字：一个服务器通常仅仅创建一个监听套接字，它在服务器的生命周期内一直存在
- 已连接套接字：内核为每个由服务器进程接受的客户连接创建一个已连接套接字，当服务器完成对某个给定客户的服务时，相应的已连接套接字就被关闭

**该函数的返回值**

有三个返回值：

- 1.新套接字描述符或出错指示的整数
- 2.客户的协议地址（cliaddr），如不关心，可置为空指针
- 3.协议地址的长度（addrlen），如不关心，可置为空指针

## 4.7 fork和exec函数

**关于fork函数**

```c
#include <unistd.h>
/*
** 作用：Unix中派生新进程的唯一方法
** 返回值：成功时，返回两次，在子进程中返回0，在父进程中返回子进程ID。若出错，返回一次，为-1
*/
pid_t fork(void);
```

**关于父子进程：**

父进程中调用fork之前打开的所有描述符在fork返回之后由子进程分享（实际上是COW机制，如果子进程需要修改，则会新拷贝一份）

fork之后，子进程会拷贝父进程的数据空间、堆和栈空间（实际上是采用写时复制技术），二者共享代码段。所以在子进程中修改全局变量（局部变量，分配在堆上的内存同样也是）后，父进程的相同的全局变量不会改变

父进程调用accept之后调用fork，所接受的已连接套接字随后就在父进程与子进程之间共享。通常情况下，子进程接着读写这个已连接套接字，父进程则关闭这个已连接套接字

**fork的两个典型用法：**

- 1.一个进程创建一个自身的副本，每个副本执行各自的操作（网络服务器的典型用法）
- 2.一个进程想要执行另外一个程序，那么它先调用fork函数创建一个自身的副本，然后调用exec函数把自身替换成新的程序（Shell之类程序的典型用法）



**关于exec函数**

可执行程序文件能够被Unix执行的唯一方法：由一个现有进程调用6个exec函数中的某一个

exec的特点：exec把当前进程映像换成新的程序文件，而且该新程序通常从main函数开始执行，进程ID并不改变。调用exec的进程称为“调用进程”，新执行的程序称为“新程序”

```c
#include <unistd.h>

//若成功则不返回，将控制权交给新程序的main函数；出错返回-1
//其他5个函数（库函数），最终都是调用execve（系统调用）
int execl (const char *pathname, const char *arg0, ... /* (char *) 0 */ );
int execv (const char *pathname, char *const argv[]);
int execle (const char *pathname, const char *arg0, .../* (char *) 0, char *const envp[] */ );
int execve (const char *pathname, char *const argv[], char *const envp[]);
int execlp (const char *filename, const char *arg0, ... /* (char *) 0 */ );
int execvp (const char *filename, char *const argv[]);
```

以上6个exec函数的区别是：

- 1.待执行的程序是由文件名还是由路径名指定 
- 2.新程序的参数是一一列出还是由一个指针数组来引用 
- 3.把调用进程的环境传递给新程序还是给新程序指定新的环境

进程在调用exec之前打开着的描述符通常跨exec继续保持打开。可以使用fcntl设置FD_CKIEXEC描述符标志禁止掉

## 4.8 并发服务器

**迭代服务器：**由主进程直接处理已连接套接字，没有使用fork

**并发服务器：**由主进程fork出子进程，由子进程处理已连接套接字

```c
pid_t pid;
int listenfd, connfd;
listenfd = Socket(AF_INET, SOCK_STREAM, 0);
Bind(listenfd,...);
Listen(listenfd, LISTENQ);
for ( ; ; ) {
    connfd = Accept(listenfd,...);
    if ( (childpid = Fork()) == 0) { //子进程
			Close(listenfd); //关闭子进程监听套接字（引用计数-1）
        	doit(connfd); //处理已连接套接字的请求
        	Close(connfd); //关闭已连接套接字，该句并非必须，因为exit时会自动处理
			exit(0); //终止子进程
    }
    Close(connfd); //父进程不等待子进程，关闭已连接套接字（引用计数-1），并继续循处理新的另一个连接
}

```

**关于Close**

父进程对connfd调用close没有终止它与客户端连接的原因：

每个文件或套接字都有一个引用计数，在文件表项中维护，是当前打开着的引用该文件或套接字的描述符的个数。如上代码所示，Socket函数返回时，listenfd关联的文件表项的引用计数为1，Accept函数返回后connfd的引用计数也为1，fork后，子进程复制父进程数据，它们的引用计数都为2。套接字真正清理和资源释放要等引用计数为0时才发生

**创建连接和子进程的过程：**

**第一步**

![](../../pics/network/unp笔记/Pic_4_14_accept返回前客户服务器的状态.png)

**第二步：**

![](../../pics/network/unp笔记/Pic_4_15_accept返回后客户服务器的状态.png)



**第三步：**

![](../../pics/network/unp笔记/Pic_4_16_fork返回后客户服务器的状态.png)

**第四步：**

![](../../pics/network/unp笔记/Pic_4_17_父进程关闭相应套接字后客户服务器的状态.png)

## 4.9 close函数

```c
#include <unistd.h>
/**
** 作用：用来关闭套接字（实际上是套接字引用计数-1），并终止TCP连接
** @sockfd：想要进行close操作的套接字描述符
** @返回值：若成功为0，出错为-1
**/
int close (int sockfd);         
```

**关闭后的套接字描述符**

关闭后的套接字描述符不能再由调用进程使用，也就是说它不能再作为read或write的第一个参数。但在这之前，会尝试发送已经排队等待发送到对端的数据，发送完这些数据后再进行正常的TCP终止序列（四次挥手）

**描述符引用计数**

**知识点1**：close只是减少引用计数，如果调用close后，引用计数仍大于0，则不引发四次挥手。如果想要某个TCP连接立马发送FIN，可以使用shutdown函数

**知识点2：**如果父进程对accept返回的已连接套接字都不调用close，会发生什么？

- 1.父进程最终将耗尽可用描述符
- 2.没有一个客户连接会被终止。子进程退出后，已连接套接字的引用计数由2变为1，父进程永远不会关闭任何已连接套接字。这妨碍TCP连接终止序列的发生，导致连接一直打开着


## 4.10 getsockname和getpeername函数

```c
#include <sys/socket.h>
/*
** 作用：返回与某个套接字关联的本地协议地址（IP地址和端口号的组合）
** @sockfd：本地套接字，该函数就是获取该套接字的本地协议地址
** @localaddr:本地协议地址指针
** @addrlen:值-结果参数
** @返回值：成功为0，失败为-1
*/
int getsockname(int sockfd, struct sockaddr *localaddr, socklen_t *addrlen);

/*
** 作用：返回与某个套接字关联的外地协议地址，应该就是与该socket连接的套接字的地址（IP地址和端口号的组合）
** @sockfd：本地套接字，该函数就是获取与该套接字相连的套接字的地址
** @localaddr:外地协议地址指针
** @addrlen:值-结果参数
** @返回值：成功为0，失败为-1
*/
int getpeername(int sockfd, struct sockaddr *peeraddr, socklen_t *addrlen);
```

# 第5章 TCP客户/服务器程序示例

[UNPv1第五章：TCP客户服务器程序实例](https://blog.csdn.net/lxj1137800599/article/details/51247971)

## 5.1 概述

回射服务器的执行步骤：

- 1.客户从标准输入读一行文本，写到服务器上
- 2.服务器读入此行，并回射给客户
- 3.客户读此回射行写到标准输出 

![](../../pics/network/unp笔记/Pic_5_1_简单的回射客户_服务器.png)

## 5.2 TCP回射服务器程序：main函数

```c++
// tcpcliserv/tcpserv01.c
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	socklen_t			clilen;
	struct sockaddr_in	cliaddr, servaddr;

	//1.创建套接字，目前为未连接套接字，Listen之后变为监听套接字
	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	//2.设置地址结构
    //2.1.将地址结构清空
	bzero(&servaddr, sizeof(servaddr));
    //2.2.设置协议族
	servaddr.sin_family      = AF_INET;
    //2.3.绑定通配地址：告知系统：要是系统是多宿主机，我们将接受目的地址为任何本地接口的连接
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);//通配地址INADDR_ANY
    //2.3.绑定端口，#define SERV_PORT 9877
	servaddr.sin_port        = htons(SERV_PORT);//端口号SERV_PORT(9877)

	//3.将套接字绑定到地址
    //#define	SA	struct sockaddr，IPv4通用套接字地址结构
	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	//4.将未连接套接字转换为监听套接字
	Listen(listenfd, LISTENQ);

	for ( ; ; ) {
		clilen = sizeof(cliaddr);
		//5.阻塞在Accept，等待来自客户端的连接
		connfd = Accept(listenfd, (SA *) &cliaddr, &clilen);
        
		//6.为每个客户派生一个处理它们的子进程，子进程会关闭监听套接字（引用计数-1），父进程关闭已连接套接字（引用计数-1），继续在监听套接字监听
		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	//关闭监听套接字（引用计数-1）
			//7.子进程调用str_echo接收并回显客户端发来的消息
			str_echo(connfd);	/* process the request */
			exit(0);
		}
		Close(connfd);		//关闭已连接套接字（引用计数-1）
	}
}
```

## 5.3 TCP回射服务器程序：str_echo函数

```c
// lib/str_echo.c
#include	"unp.h"

//作用：处理每个客户的服务：从客户读入数据，并把它们回射给客户
void
str_echo(int sockfd)
{
	ssize_t		n;
	char		buf[MAXLINE];

again:
    //read从套接字读入数据,如果客户关闭连接，那么接收到客户的FIN将导致服务器子进程的read返回0，否则返回读取到的字节数
	while ( (n = read(sockfd, buf, MAXLINE)) > 0)
        //Writen把buf中的内容回射给客户
		Writen(sockfd, buf, n);

	if (n < 0 && errno == EINTR)
		goto again;
	else if (n < 0)
		err_sys("str_echo: read error");
}
```

## 5.4 TCP回射客户程序：main函数

```c
// tcpcliserv/tcpcli01.c
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_in	servaddr;

	if (argc != 2)
		err_quit("usage: tcpcli <IPaddress>");
	
    //1.创建套接字
	sockfd = Socket(AF_INET, SOCK_STREAM, 0);
	
    //2.创建服务器地址结构
    //2.1将地址结构清空
	bzero(&servaddr, sizeof(servaddr));
    //2.2.设置协议族
	servaddr.sin_family = AF_INET;
    //2.3.绑定端口，#define SERV_PORT 9877
	servaddr.sin_port = htons(SERV_PORT); //端口号SERV_PORT(9877)
    //2.4.根据main函数参数argv[1]获取服务器ip地址
    //Inet_pton的作用：ASCII字符串--->套接字地址结构中的二进制值
	Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);
	
    //3.发起TCP 3次握手建立连接
	Connect(sockfd, (SA *) &servaddr, sizeof(servaddr));
	
    //4.从输入按行读取文本，发送至服务器，接收并显示回显
	str_cli(stdin, sockfd);		/* do it all */

	exit(0);
}
```

## 5.5 TCP回射客户程序：str_cli函数

```c
// lib/str_cli.c
#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	char	sendline[MAXLINE], recvline[MAXLINE];
	//Fgets从客户端读入一行数据
	//当遇到文件结束符或错误，fgets（库函数）将返回一个空指针，于是客户端循环终止
	//Fgets包裹fgets，检查是否发送错误，出错则终止程序，因此Fgets只有遇到文件结束符时才返回一个空指针
	while (Fgets(sendline, MAXLINE, fp) != NULL) {
		//Writen把从客户端读到的数据发送到服务端
		Writen(sockfd, sendline, strlen(sendline));
		//Readline读取从服务器回射的数据
		if (Readline(sockfd, recvline, MAXLINE) == 0)
			err_quit("str_cli: server terminated prematurely");
		//Fputs把它写到标准输出
		Fputs(recvline, stdout);
	}
}
```

## 5.6 正常启动

**启动服务器**

```shell
# 启动服务器
$ ./tcpserv01 

# 查看服务器套接字的状态
# Local Address *:9877 表示本地地址为 通配地址+9877端口
# State LISTEN 表示套接字处于监听状态
# 
$ netstat -a | grep 9877
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 *:9877                  *:*                     LISTEN
```

**启动客户端**

```shell
# 启动客户端
# 由于Fgets，进入阻塞状态
# 此时还存在另两个阻塞状态：服务器主进程阻塞与Accept，服务器子进程阻塞于readline
$ ./tcpcli01 127.0.0.1

# 查看套接字状态
# 由于服务器和客户端都在同一台主机，因此我们可以看到服务器的：监听套接字、服务器已连接套接字（第一个ESTABLISHED）和客户端的已连接套接字（第二个ESTABLISHED）
$ netstat -a | grep 9877
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 *:9877                  *:*                     LISTEN     
tcp        0      0 localhost:9877          localhost:51030         ESTABLISHED
tcp        0      0 localhost:51030         localhost:9877          ESTABLISHED

# 查看进程状态和关系
# 处于inet_csk_accept（阻塞于accept）为服务器的父进程，其PPID为1700，应该就是shell(bash)的PID
# 处于sk_wait_data（阻塞于read）为服务器的子进程，其PPID为1813，即服务器的父进程ID
# 处于wait_woken（阻塞与fgets）的为客户端进程
# STATE S+表示进程在等待某些资源而睡眠
$ ps -a -o pid,ppid,tty,stat,args,wchan
   PID   PPID TT       STAT COMMAND                     WCHAN
  1813   1700 pts/8    S+   ./tcpserv01                 inet_csk_accept
  1818   1742 pts/9    S+   ./tcpcli01 127.0.0.1        wait_woken
  1819   1813 pts/8    S+   ./tcpserv01                 sk_wait_data
```

## 5.7 正常终止

**客户端与服务器数据交换**

```shell
$ ./tcpcli01 127.0.0.1
hello,world
hello,world
```

**正常终止**

```shell
# 客户端
# 输入EOF
$ Control-D

# 查看连接状态
$ netstat -a | grep 9877
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 *:9877                  *:*                     LISTEN     
tcp        0      0 localhost:51030         localhost:9877          TIME_WAIT

# 查看进程状态
# 注意处于STAT Z+状态的进程，这是服务器子进程退出后，变为僵死状态，因为没被父进程回收
$ ps -a -o pid,ppid,tty,stat,args,wchan
   PID   PPID TT       STAT COMMAND                     WCHAN
  1813   1700 pts/8    S+   ./tcpserv01                 inet_csk_accept
  1819   1813 pts/8    Z+   [tcpserv01] <defunct>       exit
```

**正常终止客户和服务器的步骤**

- 1.键入EOF（Control-D），客户端的fgets返回一个空指针，于是从str_cli函数返回
- 2.str_cli返回到客户端的main函数，main通过调用exit(0)终止
- 3.客户端进程终止处理的部分工作是关闭所有打开的描述符，因此客户端打开的套接字由内核关闭：
  - 客户端TCP发送一个FIN给服务器（第一次挥手），客户端进入FIN_WAIT_1
  - 服务器以ACK响应（第二次挥手），服务器进入CLOSE_WAIT，客户端进入FIN_WAIT_2
- 4.服务器收到TCP的FIN时，服务器子进程阻塞与str_echo函数的readline调用，于是readline返回0，str_echo返回服务器子进程的main函数
- 5.服务器子进程通过调用exit(0)来终止
- 6.服务器子进程中打开的所有描述符随之关闭。由子进程来关闭已连接套接字，引发TCP连接终止的最后两个分节：
  - 服务器子进程发送FIN（第三次挥手），服务器进入LAST_ACK
  - 客户端发送最后的ACK（第四次挥手），客户端进入TIME_WAIT，服务器CLOSED

**除了以上步骤，需要注意的是：**

在服务器子进程终止时，给父进程发送了一个SIGCHLD信号（这里点在本例中发生了，但没有在代码中捕获该信号，而该信号的默认行为是被忽略），父进程未加处理，子进程进入僵死状态。看到下面的Z+表示子进程进入僵死状态

```shell
# 查看进程状态
# 注意处于STAT Z+状态的进程，这是服务器子进程退出后，变为僵死状态，因为没被父进程回收
$ ps -a -o pid,ppid,tty,stat,args,wchan
   PID   PPID TT       STAT COMMAND                     WCHAN
  1813   1700 pts/8    S+   ./tcpserv01                 inet_csk_accept
  1819   1813 pts/8    Z+   [tcpserv01] <defunct>       exit
```

## 5.8 POSIX信号处理

### 信号基本知识

信号（signal）：告知某个进程发生了某个事件的通知，也称为软件中断

信号通常是异步的：即进程预先不知道信号的发生时刻

信号可以：

- 1.由一个进程发送给另一个进程（或自身）
- 2.由内核发给某个进程


信号的行为：每个信号都有一个与之关联的处置，称为信号的行为。可以通过调用sigaction函数设定一个信号的行为

设置信号的行为有三种方式：

- 1.捕获信号：提供一个信号处理函数，只要有特定信号发生它就被调用
  - `void handler(int signo);`
  - 不能被捕获的信号：SIGKILL、SIGSTOP
- 2.忽略信号：把某个信号的处置设为SIG_IGN来忽略它
  - 不能被忽略的信号：SIGKILL、SIGSTOP
- 3.默认处置：把某个信号的处置设定为SIG_DEF来启用它的默认处置
  - 默认处置的行为有两种：
    - 通常是收到信号后终止进程，其中某些信号还在工作目录下生产一个进程的核心映像（core image，也称为内存影像）
    - 忽略信号，例如SIGCHLD和SIGURC的默认处置就是忽略该信号

### signal函数

建立信号处置的POSIX方法是调用sigaction函数

```c
 #include<signal.h>
/*
** @signum:要操作的信号
** @act:要设置的对信号的新处理方式，指向sigaction结构的指针
** @sigaction:原来对信号的处理方式
** @返回值：0 表示成功，-1 表示有错误发生
*/
 int sigaction(int signum,const struct sigaction *act ,struct sigaction *oldact);
```

由于该函数的act必须分配和添加，因此使用下面的signal（注意与标准C库函数中signal的区别，在一些系统中，C库函数中signal不会使得内核自动重启被中断的系统调用）对sigaction进行封装

```c
/* include signal */
#include	"unp.h"

//typedef	void	Sigfunc(int);
/*
** @signo:信号名
** @func:指向处置函数的指针、或为常值SIG_IGN、SIG_DEF
** 返回值：signal的返回值为一个Sigfunc类型的函数指针
** 这个指针指向的函数仅有一个整型参数，且没有返回值
*/
Sigfunc *
signal(int signo, Sigfunc *func)
{
	//处置结构
	struct sigaction	act, oact;

	//设置处理函数
	act.sa_handler = func;
	/*
	** 作用：设置处理函数的信号掩码
	** POSIX允许指定一组信号，它们在信号处理函数被调用时阻塞。任何阻塞的信号都不能递交给进程
	** 这里是把act.sa_mask设为空集，意味着在该信号处理函数运行期间，不阻塞额外的信号
	** 注意：这里的阻塞与之前说的阻塞于Accept函数的意义不一样
	** 这里的阻塞指：阻塞某个信号或信号集，防止它们在阻塞期间递交。反操作为：解阻塞
	** 之前的阻塞指：阻塞在某个系统调用上，这个系统调用因没有必要资源而必须等待，直到这些资源可用才返回。等待期间进程进入睡眠状态。相对的概念为：非阻塞
	*/
	sigemptyset(&act.sa_mask);
	//设置SA_RESTART标志，可选的。如果设置，由相应信号中断的系统调用将由内核自动重启
	act.sa_flags = 0;
	if (signo == SIGALRM) {
#ifdef	SA_INTERRUPT
		act.sa_flags |= SA_INTERRUPT;	/* SunOS 4.x */
#endif
	} else {
#ifdef	SA_RESTART
		act.sa_flags |= SA_RESTART;		/* SVR4, 44BSD */
#endif
	}
	//调用sigaction函数
	if (sigaction(signo, &act, &oact) < 0)
		return(SIG_ERR);
    //将相应信号的旧行为（上一次设置的行为）作为signal函数的返回值
	return(oact.sa_handler);
}
/* end signal */

//对signal再封装，多了一步错误处理
Sigfunc *
Signal(int signo, Sigfunc *func)	/* for our signal() function */
{
	Sigfunc	*sigfunc;

	if ( (sigfunc = signal(signo, func)) == SIG_ERR)
		err_sys("signal error");
	return(sigfunc);
}
```

### POSIX信号语义

信号处理的总结：

- 1.一旦安装了信号处理函数，它便一直安装着
- 2.在一个信号处理函数运行期间，正在递交的信号时阻塞的。且可以通过安装处理函数时传递给sigaction函数的sa_mask信号集中指定的额外信号也被阻塞（上面的代码中，sa_mask为空集，意味除了被捕获的信号外，没有额外信号被阻塞）
- 3.如果一个信号在阻塞期间产生了一次或多次，那么该信号被解阻塞后通常只递交一次，即Unix信号时默认不排队的
- 4.利用sigprocmask函数选择性地阻塞或解阻塞一组信号是可能的。这样可以做到一段临界区代码执行期间，防止捕获某些信号，以保护这段代码

## 5.9 处理SIGCHLD信号

僵死（zombie）状态的**目的**：维护子进程的信息，以便父进程在以后某个时候获取，这些信息包括：

- 1.子进程的进程ID
- 2.终止状态
- 3.资源利用情况（CPU时间、内存使用量）

如果一个进程终止，而该进程有子进程处于僵死状态，那么它的所有僵死进程的父进程ID将被重置为1（由init进程管理），继承这些子进程的init进程将清理它们（init进程将wait这些僵死进程，从而去除它们的僵死状态）

僵死进程的**危害**：占用内核中的空间，最终可能导致耗尽进程资源。因此fork出来的子进程一定要wait它们，以防止它们变成僵死进程

处理僵死进程的**可移植方法**：捕获SIGCHLD，并调用wait或waitpid

### 处理僵死进程

**SIGCHLD信号处理函数**

```c
#include	"unp.h"

void
sig_chld(int signo)
{
	pid_t	pid;
	int		stat;

	pid = wait(&stat);
	printf("child %d terminated\n", pid);
    //return的作用：当某个系统调用被我们编写的某个信号处理函数中断时，可以得知该系统调用具体是被哪个信号处理函数的哪个return语句中断的
	return;
}
```

**Signal函数的实现**

```c
Sigfunc *
Signal(int signo, Sigfunc *func)	/* for our signal() function */
{
	Sigfunc	*sigfunc;
	//这里使用的是标准C库函数的signal，而不是第5.8节中实现的版本
	if ( (sigfunc = signal(signo, func)) == SIG_ERR)
		err_sys("signal error");
	return(sigfunc);
}
```

**可处理僵死进程的服务器main函数**

```c
//源码 tcpcliserv/tcpserv02.c
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	socklen_t			clilen;
	struct sockaddr_in	cliaddr, servaddr;
	void				sig_chld(int);

	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);
	//实际上调用的是标准C库函数的signal，而不是第5.8节中实现的版本
    //注册信号处理函数
	Signal(SIGCHLD, sig_chld);

	for ( ; ; ) {
		clilen = sizeof(cliaddr);
		connfd = Accept(listenfd, (SA *) &cliaddr, &clilen);

		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	/* close listening socket */
			str_echo(connfd);	/* process the request */
			exit(0);
		}
		Close(connfd);			/* parent closes connected socket */
	}
}

```

**运行效果**

服务器

```shell
$ ./tcpserv02 # 启动服务器
# 客户端键入EOF之后
child 2114 terminated                 # 信号处理函数sig_chld中的输出
accpet error: Interrupted system call # main函数中止执行
```

客户端

```shell
$ ./tcpcli01 127.0.0.1 # 启动客户端
hi there               # 键入一行数据  
hi there               # 在标准输出上回射一行数据
Control-D              # 键入EOF字符
```

**键入EOF后的具体步骤**

- 1.键入EOF字符终止客户端。客户TCP发送一个FIN（第一次挥手）给服务器，服务器响应一个ACK（第二次挥手）
- 2.收到客户的FIN导致服务器TCP传递一个EOF给子进程中str_echo函数中的readline，从而子进程终止
- 3.当SIGCHLD信号递交时，父进程阻塞于accept调用。sig_chld函数（信号处理函数）执行，其wait调用取到子进程的PID和终止状态，随后是printf调用，最后返回
- 4.信号在父进程阻塞于**慢系统调用**accpet函数时由父进程捕获的，内核会使accpet返回一个EINTER错误（被中断的系统调用），而如下代码显示，父进程不处理该错误

```c
#include	"unp.h"
//父进程使用的包裹Accept函数的实现
int
Accept(int fd, struct sockaddr *sa, socklen_t *salenptr)
{
	int		n;

again:
	if ( (n = accept(fd, sa, salenptr)) < 0) {
#ifdef	EPROTO
		if (errno == EPROTO || errno == ECONNABORTED)
#else
		if (errno == ECONNABORTED)
#endif
			goto again;
		else
			err_sys("accept error");
	}
	return(n);
}
```

**运行效果中的软中断**

运行效果中最后的`accpet error: Interrupted system call`是由与被中断的系统调用（accpet）没有被处理。

以上的运行效果使用的是标准C库函数中提供的signal函数，在一些系统中，它不会使得内核自动重启被中断的系统调用（在一些系统中会）。

而5.8节中实现的signal函数，设置了SA_RESTART标志，可以应对不同的操作系统，使得所有的操作系统都会自动重启被中断的系统调用

### 处理被中断的系统调用

**慢系统调用**：可能永远阻塞的系统调用，即可能永远无法返回，多数网络支持函数都属于这一类，例如accept函数

**适用于慢系统调用的基本规则**：当阻塞于某个慢系统调用的一个进程捕获某个信号且相应处理函数返回时，该系统调用可能返回一个EINTR错误。有些内核自动重启某些软中断的系统调用，而有的却不会

为了移植性，我们编写捕获信号的程序时，必须对慢系统调用返回EINTR有所准备。下面是能正确处理EINTER错误的服务器main函数

```c
// tcpcliserv/tcpserv03.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	socklen_t			clilen;
	struct sockaddr_in	cliaddr, servaddr;
	void				sig_chld(int);

	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);

	Signal(SIGCHLD, sig_chld);

	for ( ; ; ) {
		clilen = sizeof(cliaddr);
		if ( (connfd = accept(listenfd, (SA *) &cliaddr, &clilen)) < 0) 		{
			//用户实现重启被中断的系统调用
			if (errno == EINTR)
				continue;		/* back to for() */
			else
				err_sys("accept error");
		}

		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	/* close listening socket */
			str_echo(connfd);	/* process the request */
			exit(0);
		}
		Close(connfd);			/* parent closes connected socket */
	}
}
```

可以用户实现重启被中断的系统调用：accept、read、write、select、open

不可以用户实现重启被中断的系统调用：connect

connect的处理方法：如果系统不自动重启，则调用select等待连接完成

## 5.10 wait和waipid函数

```c
#include <sys/wait.h>

/*
** 作用：处理已终止的子进程，如果调用wait时没有已终止的子进程，但有仍然在允许的子进程，则wait会阻塞到现有子进程第一个终止为止
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

### 函数wait和waipid的区别

**发现问题**

可以与服务器建立5个连接的客户端程序，目的是从并发服务器上派生多个子进程

```c
// tcpcliserv/tcpcli04.c
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					i, sockfd[5];
	struct sockaddr_in	servaddr;

	if (argc != 2)
		err_quit("usage: tcpcli <IPaddress>");

	for (i = 0; i < 5; i++) {
		sockfd[i] = Socket(AF_INET, SOCK_STREAM, 0);

		bzero(&servaddr, sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		servaddr.sin_port = htons(SERV_PORT);
		Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);

		Connect(sockfd[i], (SA *) &servaddr, sizeof(servaddr));
	}

	str_cli(stdin, sockfd[0]);		/* do it all */

	exit(0);
}
```

建立连接后如图所示：

![](../../pics/network/unp笔记/Pic_5_8_与同一个并发服务器建立5个连接的客户.png)

客户端终止，调用exit函数，5个连接基本在同一时刻终止，引发5个FIN。从而导致服务器的5个子进程基本在同一时刻终止，从而差不多同一时刻有5个SIGCHLD信号传递给父进程

![](../../pics/network/unp笔记/Pic_5_10_客户终止，关闭5个连接，终止5个子进程.png)

运行效果如下：

服务器

```shell
$ ./tcpserv03 #启动服务器

# 客户端键入EOF后
child 2411 terminated
child 2410 terminated
child 2412 terminated
child 2413 terminated
```

客户端

```shell
$ ./tcpcli04 127.0.0.1
hello
hello
Control-D

# 查看进程状态，可以看到仍然有僵死进程
$ ps -a -o pid,ppid,tty,stat,args,wchan
   PID   PPID TT       STAT COMMAND                     WCHAN
  2307   1668 pts/0    S+   ./tcpserv03                 inet_csk_accept
  2414   2307 pts/0    Z+   [tcpserv03] <defunct>       exit
```

书本中的效果是只有一个子进程被wait，还有4个僵死进程。但我的运行效果是4个子进程被wait，还有1个僵死进程。根据书中的说法，这有可能是FIN的时机没有同时到达导致的。

问题：所有5个信号都在信号处理函数执行之前产生，而信号处理函数只执行一次，因为Unix信号一般是不排队的

为了解决**遗留的僵死进程**问题，应该使用**waitpid**：在信号处理函数中循环调用waitpid（因为其在无已终止子进程时可以不阻塞，所以可以循环调用。而wait不能循环调用是因为它会阻塞）

```c
#include	"unp.h"

void
sig_chld(int signo)
{
	pid_t	pid;
	int		stat;
	//循环调用waipid，当有已终止子进程时，能马上处理
	while ( (pid = waitpid(-1, &stat, WNOHANG)) > 0)
		printf("child %d terminated\n", pid);
	return;
}
```

**最终版本的服务器main函数**

该版本的做到的：

- 1.当fork子进程时，必须捕获SIGCHLD信号
- 2.当信号捕获时，必须处理中断的系统调用
- 3.SIGCHLD的信号处理函数使用循环waitpid处理，避免留下僵死进程

```c
// 源码:tcpcliserv/tcpserv04.c
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	socklen_t			clilen;
	struct sockaddr_in	cliaddr, servaddr;
	void				sig_chld(int);

	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);

	//1.、3.捕获SIGCHLD信号,并使用循环waitpid处理
	Signal(SIGCHLD, sig_chld);	/* must call waitpid() */

	for ( ; ; ) {
		clilen = sizeof(cliaddr);
		if ( (connfd = accept(listenfd, (SA *) &cliaddr, &clilen)) < 0) {
			//2.处理SIGCHLD信号发生导致的系统调用中断
			if (errno == EINTR)
				continue;		/* back to for() */
			else
				err_sys("accept error");
		}

		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	/* close listening socket */
			str_echo(connfd);	/* process the request */
			exit(0);
		}
		Close(connfd);			/* parent closes connected socket */
	}
}
```

## 5.11 accept返回前连接中断

**accept返回错误的情况：**

- 1.捕获某个信号且相应处理函数返回时，该系统调用可能返回一个EINTR错误（如SIGCHLD信号）
- 2.ESTABLISHED状态的连接在调用accpet之前收到来自客户的RST，如图所示：

![](../../pics/network/unp笔记/Pic_5_13_ESTABLISHED状态的连接在调用accpet之前收到RST.png)

如何处理这种情况依赖于不同的实现：

- 1.Berkely版本：在内核中处理终止的连接，服务器进程看不到
- 2.SVR4版本：accpet返回一个EPROTO错误（“protocol error”，协议错误）
  - 解决方法：再次调用accept
- 3.POSIX：accpet返回一个ECONNABORTED错误（“software caused connection abort”）
  - 解决方法：再次调用accept

## 5.12 服务器进程终止

**客户端的str_cli函数**

```c
// lib/str_cli.c
#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	char	sendline[MAXLINE], recvline[MAXLINE];
	//Fgets从客户端读入一行数据
	//当遇到文件结束符或错误，fgets（库函数）将返回一个空指针，于是客户端循环终止
	//Fgets包裹fgets，检查是否发送错误，发生则终止程序，因此Fgets只有遇到文件结束符时才返回一个空指针
	while (Fgets(sendline, MAXLINE, fp) != NULL) {
		//Writen把从客户端读到的数据发送到服务端
		Writen(sockfd, sendline, strlen(sendline));
		//Readline读取从服务器回射的数据
		if (Readline(sockfd, recvline, MAXLINE) == 0)
			err_quit("str_cli: server terminated prematurely");
		//Fputs把它写到标准输出
		Fputs(recvline, stdout);
	}
}
```

**问题描述**：处于ESTABLISHED的连接，服务器子进程崩溃（或被主动关闭），服务器子进程终止，并关闭所有打开着的描述符，导致向客户端发送FIN，而客户端则回应服务器ACK。此时由于客户端阻塞与Fgets而无法得知服务器已经发送了FIN，此时如果客户端再键入数据（向一个已经收到FIN的套接字发送数据，将收到一个RST），发送给服务器，服务器由于该连接的套接字已经终止，因此回应RST，之后，将会发生以下两种情况：

- 1.Readline函数发生在RST到达之前：那么Readline函数因为服务器的FIN（子进程终止时发送的）而返回0，即收到一个未预期的EOF，因此err_quit退出，并打印"str_cli: server terminated prematurely"（服务器过早终止）
- 2.Readline函数发生在RST到达之后：Readline中的readline返回一个ECONNRESET（"connection reset by peer"，对方复位连接错误），此时不会退出（Readline出错返回到str_cli，继续循环Fgets）。如果再向收到RST的套接字写数据，才会发生内核向进程发送SIGPIPE信号，并导致默认退出

**问题本质**：当FIN到达套接字时，客户正阻塞与Fgets调用上。客户实际上应该对两个描述符（套接字和用户输入）进行监听，而不能阻塞于其中一个

**解决方法**：可以通过select和poll来解决，使用这两个函数后，一旦杀死服务器子进程，客户就理解被告知收到FIN

## 5.13 SIGPIPE信号

当一个进程向某个已经收到RST的套接字执行写操作时，内核向该进程发送一个SIGPIPE信号。该信号的默认行为是终止进程，如果进程不希望被终止，需要捕获该信号

**可能出错的情景：**在readline之前，执行两次或更多的write操作。因为第一次write之前，服务器已经半关闭，第一次write（向一个已经收到FIN的套接字写）将收到RST，而继续write（向一个已经收到RST的套接字写），则会引发SIGPIPE错误

**注意：**写一个已经收到FIN的套接字是没有问题的，此时服务器会回应RST，再次写一个收到RST的套接字，将引发SIGPIPE信号

**处理SIGPIPE的方法：**

- 1.如果没有特殊的事情要做，则将信号处理处理办法设为SIG_IGN，并假设后续的输出操作将捕获EPIPE错误并终止
- 2.如果信号出现时需要采取措施（如记录日志），就需要捕获信号，并编写信号处理函数

## 5.14 服务器主机崩溃

**服务器主机崩溃（或网络不可达）：**区别于“服务器进程终止（会发送FIN）”和“服务器因操作员执行命令关机（会发送FIN）”。崩溃应该是指死机、拔网线、拔电线一类的（不发送FIN）

**过程描述：**

- 1.客户与服务器建立连接，处于ESTABLISHED状态


- 2.服务器主机崩溃，不发送FIN
- 3.客户键入文本，调用Writen写入内核，并由客户TCP发送分节
- 4.客户阻塞于readline，等待回射的应答
- 5.客户TCP持续重传数据分组，试图从服务器上收到ACK
- 6.客户TCP放弃，给客户进程返回一个错误，此时客户仍然阻塞于readline，readline将返回该错误

**错误的类别：**

根据情况不同，有可能发生不同的错误

- 1.如果某个中间路由器判定服务器主机已经不可达，则路由向客户主机发送"destination unreachable"（目的地不可达）ICMP消息，那么客户TCP向客户进程返回的错误是EHOSTUNREACH或ENETUNREACH
- 2.服务器主机崩溃，客户的数据分节没有响应，返回错误ETIMEDOUT（超时）

**解决方法：**

在本例中，只有我们向服务器发送数据时才能检测出它已经崩溃，如果不主动发送也想检测到服务器主机崩溃，可以使用SO_KEEPLIVE（心跳机制，7.5节介绍）

## 5.15 服务器主机崩溃后重启

**前提：**不使用SO_KEEPLIVE，即客户不主动给服务器发送数据，客户将不会知道服务器主机已经崩溃

**过程描述：**

- 1.客户与服务器建立连接，处于ESTABLISHED状态
- 2.服务器主机崩溃并重启
- 3.客户键入文本，调用Writen写入内核，并由客户TCP发送分节
- 4.客户阻塞于readline，等待回射的应答
- 5.当服务器重启好后，服务器TCP丢失了崩溃前所有的连接信息，因此服务器TCP收到客户的数据分节响应以RST
- 6.当客户TCP收到服务器的RST，此时客户阻塞于readline，导致该调用返回ECONNRESET错误（"connection reset by peer"，对方复位连接错误），此时不会退出（Readline出错返回到str_cli，继续循环Fgets）。如果再向收到RST的套接字写数据，才会发生内核向进程发送SIGPIPE信号，并导致默认退出

## 5.16 服务器主机关机

**过程描述**

- 1.Unix系统关机
- 2.init进程向所有进程发送SIGTERM信号（该信号可被捕获）
  - 若不忽略该信号，该信号默认行为是终止进程，进程终止后，跳到第5步
  - 若忽略该信号，继续第3步
- 3.等待一段固定的时间（5到20秒）
- 4.给所有仍然在运行的进程发送SIGKILL信号（该信号不能被捕获），所有进程被终止
- 5.服务器进程终止时，它将关闭所有打开着的描述符，发送FIN

## 5.18 数据格式

### 5.18.1 例子：在客户与服务器之间传递文本串

**要求：**

- 1.客户端：键入包含空格分开的两个整数
- 2.服务器：返回客户端输入的两个整数的和

需要修改的只是服务器的str_echo函数

```c
// 源码：tcpcliserv/str_echo08.c
#include	"unp.h"

void
str_echo(int sockfd)
{
	long		arg1, arg2;
	ssize_t		n;
	char		line[MAXLINE];

	for ( ; ; ) {
		if ( (n = Readline(sockfd, line, MAXLINE)) == 0)
			return;		/* connection closed by other end */
		//sscanf把文本串中的两个参数转换为长整数
		if (sscanf(line, "%ld%ld", &arg1, &arg2) == 2)
			//snprintf把结果转换为文本串
			snprintf(line, sizeof(line), "%ld\n", arg1 + arg2);
		else
			snprintf(line, sizeof(line), "input error\n");

		n = strlen(line);
		Writen(sockfd, line, n);
	}
}
```

### 5.18.2 例子：在客户与服务器之间传递二进制结构

**要求**：客户和服务器穿越套接字传递二进制值

**三个潜在问题：**

- 1.不同的实现以不同的格式存储二进制数
  - 如使用大端字节序或小端字节序
- 2.不同的实现在存储相同的C数据类型上可能存在异常
  - 大多数32位Unix系统使用32位表示长整数，而64位系统却使用64位来表示长整数
  - short、int或long等整数类型，各自的大小也都没有明确的保证
- 3.不同的实现给结构打包的方式存在差异
  - 打包方式取决于各种数据类型所使用的位数以及机器的对齐限制

**解决方法**

- 1.把所有的数值数据作为文本串来传递（前提是客户与服务器主机具有相同的字符集）
- 2.显示定义所支持数据类型的二进制格式（位数、大端或小端字节序），并以这样的格式在客户与服务器之间传递所有数据
  - 远程过程调用软件包通常使用这种技术

**问题描述**

客户和服务器的main函数不需要修改，只需修改客户的`str_cli`函数和服务器的`str_echo`函数，同时，为参数和结果定义结构

```c
// 源码: tcpcliserv/sum.h
//为参数和结果定义结构
struct args {
  long	arg1;
  long	arg2;
};

struct result {
  long	sum;
};
```

**客户端str_cli**

```c
// 源码：tcpcliserv/str_cli09.c

#include	"unp.h"
#include	"sum.h"

void
str_cli(FILE *fp, int sockfd)
{
	char			sendline[MAXLINE];
	struct args		args;
	struct result	result;

	while (Fgets(sendline, MAXLINE, fp) != NULL) {
		//sscanf把两个参数从文本串转换为二进制数
		if (sscanf(sendline, "%ld%ld", &args.arg1, &args.arg2) != 2) {
			printf("invalid input: %s", sendline);
			continue;
		}
		//将参数数据结构发送给服务器
		Writen(sockfd, &args, sizeof(args));
		//读取服务器回应
		if (Readn(sockfd, &result, sizeof(result)) == 0)
			err_quit("str_cli: server terminated prematurely");
		//输出结果
		printf("%ld\n", result.sum);
	}
}
```

**服务器str_echo**

```c
// 源码：tcpcliserv/str_echo09.c

#include	"unp.h"
#include	"sum.h"

void
str_echo(int sockfd)
{
	ssize_t			n;
	struct args		args;
	struct result	result;

	for ( ; ; ) {
		//读入参数
		if ( (n = Readn(sockfd, &args, sizeof(args))) == 0)
			return;		/* connection closed by other end */
		//计算和
		result.sum = args.arg1 + args.arg2;
		//把结果结果发回客户
		Writen(sockfd, &result, sizeof(result));
	}
}
```

**结果展示**

情况一：如果客户和服务器具有相同的体系结构，不会存在什么问题

```shell
$ tcpcli09 12.106.32.254
11 22                  #客户键入两个数
33                     #服务器应答
-11 -44                #客户再次键入两个数
-55                    #服务器应答
```

情况二：如果客户和服务器具有不同的体系结构，则无法正常工作

```shell
$ tcpcli09 12.106.32.254
1 2                    #客户键入两个数
3                      #服务器应答，结果正确
-22 -77                #客户再次键入两个数
-16777314              #服务器应答，结果错误
```

# 第6章 I/0复用：select和poll函数

内核一旦发现进程指定的一个或多个I/O条件就绪（也就是说输入已准备好被读取，或描述符已能承接更多的输出），它就通知进程，这个能力称为I/O复用，是由select、poll、pselect、epoll函数支持的

I/O复用适合的网络应用场景：

- 1.当客户处理多个描述符（交互式输入和网络套接字）时
- 2.一个客户同时处理多个套接字
- 3.TCP服务器既要处理监听套接字，又要处理已连接套接字
- 4.服务器既要处理TCP，又要处理UDP
- 5.服务器要处理多个服务或者多个协议

I/O复用并非只限于网络编程，许多重要的应用程序也需要使用这项技术

## 6.2 I/O模型

**五种I/O模型**：阻塞式I/O、非阻塞I/O，I/O复用，信号驱动式I/O，异步I/O

对于一次IO访问（以read举例），数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。所以说，当一个read操作发生时，它会经历两个阶段：

- 等待数据准备 (Waiting for the data to be ready)：数据拷贝到操作系统内核的缓冲区中
  - 涉及等待数据从网络中到达，当所等待分组到达时，它被复制到内核中的某个缓冲区
- 将数据从内核拷贝到进程中 (Copying the data from the kernel to the process)
  - 把数据从内核缓冲区复制到应用进程缓冲区

**下面的例子环境说明：**

基于udp进行说明，因为概念较简单

### 6.2.1 阻塞式I/O模型

![](../../pics/network/unp笔记/Pic_6_1_阻塞式IO模型.png)

默认情况下，所有套接字都是阻塞的

把recvfrom视为系统调用

进程调用recvfrom，其系统调用直到

- 1.数据报到达且被复制到应用进程的缓冲区中
- 2.发生错误

才返回

### 6.2.2 非阻塞式I/O模型

![](../../pics/network/unp笔记/Pic_6_2_非阻塞式IO模型.png)

把一个套接字设为非阻塞是在通知内核：当所请求的I/O操作非得把本进程投入睡眠才能完成时，不把本进程投入睡眠，而是返回一个错误（EWOULDBLOCK）

如果调用recvfrom已有一个数据报准备好，它将被复制到应用进程缓冲区，然后recvfrom成功返回

这种方式也称为**轮询**：应用进程持续轮询内核，以查看某个进程是否就绪。这样做往往消耗大量CPU时间

### 6.2.3 I/O复用模型

![](../../pics/network/unp笔记/Pic_6_3_IO复用模型.png)

有了I/O复用模型，就可以调用select或poll并阻塞在这两个系统调用中，而不是阻塞在真正的I/O系统调用上

执行步骤：

- 1.阻塞于select调用，等待数据报套接字变为可读
- 2.select返回套接字可读
- 3.调用recvfrom把所读数据报复制到应用进程缓冲区

### 6.2.4 信号驱动式I/O模型

![](../../pics/network/unp笔记/Pic_6_4_信号驱动式IO模型.png)

**步骤：**

- 1.开启套接字的信号驱动式I/O功能（在25.2节讲解）
- 2.通过sigaction系统调用安装一个信号处理函数，该系统调用立即返回
- 3.进程继续工作，不阻塞
- 4.当数据准备好读取时（在内核缓冲区中），内核为该进程产生一个SIGIO信号
- 5.接着，可以有两种方式：
  - 5.1.信号处理函数中调用recvfrom读取数据报，并通知主循环数据已经准备好待处理
  - 5.2.立即通知主循环，让它读取数据报

### 6.2.5 异步I/O模型

![](../../pics/network/unp笔记/Pic_6_5_异步IO模型.png)

**工作机制**：进程告知内核启动某个操作，并让内核在整个操作（包括将数据从内核复制到进程缓冲区）完成后再通知进程

**信号驱动式I/O模型与异步I/O模型的主要区别**：信号驱动式I/O由内核通知进程**何时启动**一个I/O操作（recvfrom），而异步I/O模型由内核通知进程I/O操作**合适完成**

**步骤：**

- 1.调用aio_read函数，给内核传递描述符、缓冲区指针、缓冲区大小、文件偏移，并告知内核当整个操作完成时通知进程。该系统调用立即返回
- 2.在等待I/O期间，进程不阻塞
- 3.数据复制到应用程序缓冲区后，内核向进程发送某个信号

**注意**：本书编写的时候，支持POSIX异步I/O模型的系统仍较罕见

### 6.2.6 各种I/O模型的比较

**各种模型的区别**

- 前4种之间的区别：第一阶段是否阻塞；他们的第二阶段都是一样的（阻塞）
- 同步（前4种）与异步（第5种）的区别：第二阶段是否阻塞（真正的I/O操作：recvfrom阶段是否阻塞）

![](../../pics/network/unp笔记/Pic_6_6_5种IO模式的比较.png)

## 6.3 select函数

```c
#include <sys/select.h>
#include <sys/time.h>

int select(int maxfpd1, fd_set *readset, fd_set *writeset, fd_set *exceptset, const struct timeval *timeout);
```

**参数解释**

- **timeout:**

作用：告知内核等待所指定描述符中的任何一个就绪可花多长时间

```c
struct timeval {
    long tv_sec; //秒数
    long tv_usec; //微秒
};
```

timeout参数有**三种可能**

- 1.空指针：永远等待，仅在一个描述符准备好I/O时才返回
- 2.timeval指定秒数和微秒数：等待设定的时间，在有一个描述符准备好I/O才返回
- 3.timeval的两个变量值为0：不等待，检查描述符后立即返回，也称为轮询

select**返回的情况**

- 1.一个或多个描述符就绪
- 2.等待期间，从信号处理函数返回时，得到EINTR错误（永远等待、等待设定时间情况下可能发生）

timeout中**const表示**：timeout中的变量值不会被select修改为剩余时间



- **readset、writeset、expectset：**

表示让内核测试读就绪、写就绪和异常条件的描述符集

异常条件有：

- 1.某个套接字的带外数据的到达（第24章）
- 2.某个已置为分组模式的伪终端存在可从其主端读取的控制状态信息

三个中，如果对某一个条件不感兴趣，可以设为空指针。如果三个都设为空指针，select就成为一个比Unix的sleep函数更精确的定时器

三个都是**值-结果参数**：

- 1.调用函数时：指定所关心的描述符的值
- 2.函数返回时：描述符就绪，则设为1；未就绪，则设为0
- 3.函数返回后：使用`FD_ISSET`宏来测试`fd_set`数据类型中的描述符
- 4.每次重新调用select时，需要再次把描述符集内所关心的位设为1

描述符集的实质是：整数数组，每一位对应一个描述符

**相关操作**

```c
void FD_ZERO(fd_set *fdset); //清除描述符集中的所有位
void FD_SET(int fd, fd_set *fdset); //将fd描述符的位设为1，表示对此感兴趣
void FD_CLR(int fd, fd_set *fdset); //将fd描述符的位设为0，表示对此不感兴趣
void FD_ISSET(int fd, fd_set *fdset); //测试fd描述符是否为1
```

**例子**

```c
fd_set rset;
FD_ZERO(&rset); //初始化，一定要做，否则后果不可预期
FD_SET(1,&rset); //对1描述符设为感兴趣
FD_SET(4,&rset); //对4描述符设为感兴趣
FD_SET(5,&rset); //对5描述符设为感兴趣
```



- **maxfdp1**

指定待测试的描述符个数，它的值是最大描述符加1（并不只是值为1的个数），例如打开描述符1、4、5，则maxfdp1的值是6（即[0,5]范围有几个描述符）。这样做的目的是为了效率，maxfdp1之后的描述符可以不轮训

### 6.3.1 描述符就绪条件

**套接字准备好读**

- 1.套接字接收缓冲区中的数据字节大于等于套接字接收缓冲区低水位标记的当前大小
  - 返回情况：不阻塞，返回一个大于0的值（返回准备好读入的数据大小）
  - 低水平标记：使用SO_RCVLOWAT套接字选项设置，TCP、UDP默认为1
- 2.套接字连接读半部关闭（接收了对端的FIN的TCP连接）
  - 返回情况：不阻塞，返回0（即EOF）
- 3.套接字是一个监听套接字，已完成连接数不为0
  - 返回情况：accept函数不阻塞，返回一个已连接套接字的sockfd
- 4.有套接字错误待处理
  - 返回情况：不阻塞，返回-1，把errno设为错误条件
  - 获取错误方式：
    - errno
    - 指定SO_ERROR套接字选项调用getsockopt获取并清除

**套接字准备好写**

- 1.套接字发送缓冲区中的可用空间字节数大于等于套接字发送缓冲区低水位标记的当前大小
  - 套接字条件：已连接（TCP），不需要连接（UDP）
  - 返回情况：如果套接字设为非阻塞，不阻塞，返回一个正值（由传输层可接受的字节数）
  - 低水平标记：使用SO_SNDLOWAT套接字选项设置，TCP、UDP默认为1024
- 2.套接字连接写半部关闭（本端已经发送FIN）
  - 对写半关闭的套接字写，将产生SIGPIPE信号（对于收到RST的套接字写也会产生该信号）
- 3.使用非阻塞式connect的套接字已建立连接，或connect已经以失败告终
- 4.有套接字错误待处理
  - 返回情况：不阻塞，返回-1，把errno设为错误条件
  - 获取错误的方式：
    - errno
    - 指定SO_ERROR套接字选项调用getsockopt获取并清除
- 5.如果一个套接字存在带外数据或仍处于带外标记，那么它有异常条件待处理

**注意**：当某个套接字发生错误时，它将由select标记为既可读又可写

**低水位标记**

- 接收低水位标记和发送低水位标记的**目的**：允许应用进程控制在select返回可读或可写条件之前有多少数据可读或有多大空间可用于写
- 任何UDP套接字只要其发送低水位标记小于等于发送缓冲区大小，则总是可以写，因为UDP不需要连接

![](../../pics/network/unp笔记/Pic_6_7_select返回某个套接字就绪的条件小结.png)

**select的优缺点**

优点：

- **跨平台支持好**，目前几乎在所有平台上支持

缺点：

- **最大的缺点是，进程打开的fd有限**（由FD_SETSIZE和内核决定，一般为1024），这对于连接数量比较大的服务器来说根本不能满足（可以选择多进程的解决方案，虽然Linux上创建进程的代价比较小，但也不可忽视，加上进程间数据同步远比不上线程间同步的效率，所以也不是一直完美的方案）
- **函数返回后需要轮询描述符集，寻找就绪描述符，效率不高**
- **用户态和内核态传递描述符结构时copy开销大**

### 6.3.2 select的最大描述符数

**每个进程使用描述符的限制**：数目无上限，只受限于内存总量和管理性限制

select最大限制：1024个（32位系统）

相关文件与宏

```c
#include <sys/types.h> //或#include <sys/select.h>
#ifndef FD_SETSIZE
#define FD_SETSIZE 256 //但但修改这个值是无法扩展select描述符数的最大限制的
#endif
```

## 6.4 str_cli函数（select修订版1）

调用select所处理的各种条件

- 1.fgets调用
- 2.客户套接字

![](../../pics/network/unp笔记/Pic_6_8_str_cli函数中由select处理的各种条件.png)

如图的客户**套接字**的三个条件处理如下：

- 1.对端TCP发送数据，该套接字变为可读，read返回一个大于0的值（读入数据的字节数）
- 2.对端TCP发送FIN（对端进程终止），该套接字变为可读，read返回0（EOF）
- 3.对端TCP发送RST（对端主机崩溃并重启），该套接字变为可读，read返回-1，errno设为错误码

```c
// select/strcliselect01.c
#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	int			maxfdp1;
	fd_set		rset;
	char		sendline[MAXLINE], recvline[MAXLINE];
	//初始化描述符集
	FD_ZERO(&rset);
	for ( ; ; ) {
		//fileno(fp)把标准I/O文件指针fp转换为对应的描述符
		FD_SET(fileno(fp), &rset);
		//将套接字描述符设为关心
		FD_SET(sockfd, &rset);
		//获取两个描述符中的较大值，并+1计算出maxfdp1值
		maxfdp1 = max(fileno(fp), sockfd) + 1;
		//包裹函数Select，增加了返回值小于0的错误处理
		//将readset和writeset设为NULL，表示不感兴趣
		//将timeout设为NULL，表示永远等待，直到有描述符准备好
		Select(maxfdp1, &rset, NULL, NULL, NULL);
		//如果套接字可读
		if (FD_ISSET(sockfd, &rset)) {	/* socket is readable */
			//读取套接字
			if (Readline(sockfd, recvline, MAXLINE) == 0)
				//如果套接字返回0，表示对端进程终止，输出错误并退出客户进程
				err_quit("str_cli: server terminated prematurely");
			//输出套接字读入的结果
			Fputs(recvline, stdout);
		}
		//如果标准输入可读
		if (FD_ISSET(fileno(fp), &rset)) {  /* input is readable */
			//读取标准输入
			if (Fgets(sendline, MAXLINE, fp) == NULL)
                //如果为NULL表示读到EOF,退出客户进程
				return;		/* all done */
			//将标准输入读到的内容写入套接字
			Writen(sockfd, sendline, strlen(sendline));
		}
	}
}
```

## 6.5 批量输入

批量输入时，6.4节版本的str_cli函数存在的问题：

- 1.批量方式下，标准输入中的EOF并不意味着同时也完成了从套接字的读入。可能仍有请求在去往服务器的路上，或有应答在返回客户的路上
  - 解决方法：写半关闭，当标准输入读到EOF时，给服务器发送FIN，告诉它我们完成了数据发送，但仍然保持套接字描述符打开以便读取
  - 解决函数：shutdown
- 2.客户进程使用文本作为输入（标准输入）时，fgets函数读取输入时，文本输入行被读入到stdio所用的缓冲区中，而fgets只返回其中一行，其余输入仍在stdio缓冲区中。write只将其中一行写给服务器，随后select再次被调用而等待新的工作，而不管stdio缓冲区中还有额外的输入待消费
  - 原因：select不知道stdio使用了缓冲区，它只从read系统调用的角度考虑是否有数据可读，而不是在fgets的角度考虑（//TODO：read和fgets的区别是什么）
- 3.客户进程从套接字读取时，在readline调用时，套接字缓冲区也存在2.中提到的问题

这里提到的三个问题将在6.7节中的str_cli函数（select修订版2）中解决

## 6.6 shutdown函数

close函数有两个限制，可以使用shutdown来避免

close的限制：

- 1.close把描述符的引用计数-1，仅在计数变为0时才关闭套接字。shutdown可以不管引用计数，直接激发TCP的正常连接终止序列
- 2.close终止读和写两个方向的数据传送。shutdown可以告知对端已经完成了数据发送，但仍然可以接收对方发送的数据

![](../../pics/network/unp笔记/Pic_6_12_调用shutdown关闭一半TCP连接.png)

```c
#include <sys/socket.h>
//返回：成功返回0，出错返回-1
int shutdown(int sockfd, int howto);
```

**howto参数的解释**

- 1.**SHUT_RD**：关闭连接的读，套接字不再有数据可接收
  - 套接字接收缓冲区中的现有数据被丢弃
  - 进程不能再对这样的套接字调用任何读函数
  - 由该套接字接收的来自对端的任何数据都被确认，然后丢弃
- 2.**SHUT_WR**：关闭连接的写，对于TCP套接字，也称为**半关闭**
  - 套接字发送缓冲区中的数据被发送，接着发送TCP的终止序列FIN
  - 该方式不管套接字引用计数是否为0
  - 进程不能对该套接字调用任何写函数
- 3.**SHUT_RDWR**:先关闭连接的读，再关闭连接的写
  - 等效于连接先后调用1.2.

（//TODO:这张图中关于close的部分还需要去了解）

![](../../pics/network/unp笔记/Pic_7_12_shutdown和SO_LINGER各种情况的总结.png)

## 6.7 str_cli函数（select修订版2）

解决6.5节中在批量数据下提出的三个问题：输入的EOF不代表进程结束问题、标准输入（文件输入）缓冲区问题、套接字接收缓冲区问题

```c
// 源码：select/strcliselect02.c
#include	"unp.h"

void
str_cli(FILE *fp, int sockfd)
{
	//当标准输入键入EOF时，表示不会再有新的内容发往服务器
	//此时客户只需要等待所有“回射”的数据都被接收到后，就能
	//返回，stdineof就是记录是否已经键入EOF
	int			maxfdp1, stdineof;
	fd_set		rset;
	char		buf[MAXLINE];
	int		n;

	stdineof = 0;
	FD_ZERO(&rset);
	for ( ; ; ) {
		//如果标准输入EOF标记不为0，则对标准输入描述符设为关心
		if (stdineof == 0)
			FD_SET(fileno(fp), &rset);
		FD_SET(sockfd, &rset);
		maxfdp1 = max(fileno(fp), sockfd) + 1;
		Select(maxfdp1, &rset, NULL, NULL, NULL);

		if (FD_ISSET(sockfd, &rset)) {	/* socket is readable */
			//如果套接字遇到EOF
			//使用Read直接对套接字缓存进行操作，上一版本使用Readline
			if ( (n = Read(sockfd, buf, MAXLINE)) == 0) {
				//如果标准输入也已经EOF，则表示正常的终止
				if (stdineof == 1)
					return;		/* normal termination */
				else //否则，标准输入没有遇到EOF，说明服务器进程提前终止
					err_quit("str_cli: server terminated prematurely");
			}

			Write(fileno(stdout), buf, n);
		}

		if (FD_ISSET(fileno(fp), &rset)) {  /* input is readable */
			//如果标准输入遇到EOF
			//使用Read直接对缓冲区进行操作，上一版本使用Fgets
			if ( (n = Read(fileno(fp), buf, MAXLINE)) == 0) {
				//将标准输入EOF标记设为1
				stdineof = 1;
				//写端关闭
				Shutdown(sockfd, SHUT_WR);	/* send FIN */
				//取消对标准输入描述符的关心
				FD_CLR(fileno(fp), &rset);
				continue;
			}

			Writen(sockfd, buf, n);
		}
	}
}
```

**比较Read、Realine、Fgets**

Read的实现

```c
// lib/wrapunix.c
ssize_t
Read(int fd, void *ptr, size_t nbytes)
{
	ssize_t		n;

	if ( (n = read(fd, ptr, nbytes)) == -1)
		err_sys("read error");
	return(n);
}
```

Realine的实现

```c
// lib/readline.c
ssize_t
Readline(int fd, void *ptr, size_t maxlen)
{
	ssize_t		n;

	if ( (n = readline(fd, ptr, maxlen)) < 0)
		err_sys("readline error");
	return(n);
}
```

Fgets的实现

```c
// lib/wrapstdio.c
char *
Fgets(char *ptr, int n, FILE *stream)
{
	char	*rptr;

	if ( (rptr = fgets(ptr, n, stream)) == NULL && ferror(stream))
		err_sys("fgets error");

	return (rptr);
}
```

归根结底是read、readline和fgets的区别

[gets、fgets、puts、fputs、scanf、read、readline、getline等](https://blog.csdn.net/taozhi20084525/article/details/26606149)

- fgets:
  - 当遇到换行符或者缓冲区已满，fgets就会停止，返回读到的数据。注意换行符会被保存在缓冲区中，只是后面再加个'\0'
  - 对于fgets来说，'\n'是一个特别的字符，而'\0'并无任何特别之处，如果读到'\0'就当作普通字符读入。如果文件中存在'\0'字符（或者0x00字节），调用fgets之后就无法判断缓冲区中的'\0'究竟是从文件读上来的字符还是由fgets自动添加的结束符，所以fgets只适合读文本文件而不适合读二进制文件，并且文本文件中的所有字符都应该是可见字符，不能有'\0'。
- readline库:
  - readline 是一个强大的库，只要使用了它的程序，都可以用同一个配置文件配置，而且用同样的方法操作命令行，让你可以方便的编辑命令行
- read
  - 是一个系统调用，其他函数会调用该函数
  - read 函数从 filedes 指定的已打开文件中读取 nbytes 字节到 buf 中


## 6.8 TCP回射服务器程序（select修订版）

使用select重写TCP回射服务器程序，可以不使用多进程的方式处理任意个客户连接

```c
// 源码：tcpcliserv/tcpservselect01.c

/* include fig01 */
#include	"unp.h"

int
main(int argc, char **argv)
{
	/*
	* @maxi:client数组当前使用项的最大下标
	* @maxfd：目前select监听的最大描述符
	*/
	int					i, maxi, maxfd, listenfd, connfd, sockfd;
	/*
	* @nready:本次调用select返回了几个就绪描述符
	* @client[FD_SETSIZE]：存储客户套接字的数组
	*/
	int					nready, client[FD_SETSIZE];
	ssize_t				n;
	fd_set				rset, allset;
	char				buf[MAXLINE];
	socklen_t			clilen;
	struct sockaddr_in	cliaddr, servaddr;

	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);

	maxfd = listenfd;			/* initialize */
	maxi = -1;					/* index into client[] array */
	for (i = 0; i < FD_SETSIZE; i++)
		client[i] = -1;			/* -1 indicates available entry */
	FD_ZERO(&allset);
	FD_SET(listenfd, &allset);
/* end fig01 */

/* include fig02 */
	for ( ; ; ) {
		rset = allset;		/* structure assignment */
		nready = Select(maxfd+1, &rset, NULL, NULL, NULL);

		//如果监听套接字就绪
		if (FD_ISSET(listenfd, &rset)) {	/* new client connection */
			clilen = sizeof(cliaddr);
			connfd = Accept(listenfd, (SA *) &cliaddr, &clilen);
#ifdef	NOTDEF
			printf("new client: %s, port %d\n",
					Inet_ntop(AF_INET, &cliaddr.sin_addr, 4, NULL),
					ntohs(cliaddr.sin_port));
#endif

			//将新的客户套接字存入客户套接字数组
			for (i = 0; i < FD_SETSIZE; i++)
				if (client[i] < 0) {
					client[i] = connfd;	/* save descriptor */
					break;
				}
			//套接字数组已满
			if (i == FD_SETSIZE)
				err_quit("too many clients");

			FD_SET(connfd, &allset);	/* add new descriptor to set */
			if (connfd > maxfd)
				maxfd = connfd;			/* for select */
			if (i > maxi)
				maxi = i;				/* max index in client[] array */

			//如果本次select只有监听套接字就绪，直接重新调用select		
			if (--nready <= 0)
				continue;				/* no more readable descriptors */
		}

		//处理已连接套接字
		for (i = 0; i <= maxi; i++) {	/* check all clients for data */
			if ( (sockfd = client[i]) < 0)
				continue;
			if (FD_ISSET(sockfd, &rset)) {
				//已连接套接字读到EOF
				if ( (n = Read(sockfd, buf, MAXLINE)) == 0) {
						/*4connection closed by client */
					Close(sockfd);
					FD_CLR(sockfd, &allset);
					client[i] = -1;
				} else
					Writen(sockfd, buf, n);

				if (--nready <= 0)
					break;				/* no more readable descriptors */
			}
		}
	}
}
/* end fig02 */
```

**本方案优缺点：**

**优点**：避免为每个客户创建一个新进程的所有开销

**缺点**：拒绝服务型攻击





**拒绝服务型攻击：**

情景描述：如果一个恶意客户连接到服务器，发送一个字节的数据（不是换行符）后进入睡眠。服务器将调用read，从客户读入这个字节的数据，然后阻塞于下一个read调用，以等待来自该客户的其余数据。服务器于是因该客户而被阻塞（或挂起），不能再为其他任何客户提供服务（接受新客户连接或读取现有客户的数据），直到那个恶意客户发送一个换行符或终止为止

当一个服务器处理多个客户时，绝不能阻塞于只与单个客户相关的某个函数调用，否则可能导致服务器被挂起，拒绝为所有其他客户提供服务，这就是**拒绝服务型攻击**

解决方法：

- 1.使用非阻塞I/O
- 2.让每个客户单独的控制线程提供服务（例如为每个客户创建一个子进程或一个线程）
- 3.对I/O操作设置一个超时

## 6.9 pselect函数

pselect函数由POSIX发明

```c
#include <sys/select.h>
#include <signal.h>
#include <time.h>

int pselect(int maxfd1,fd_set *readset,fd_set *writeset,fd_set *exceptset, const struct timespec *timeout,const sigset_t *sigmask);
```

**与select的区别**：

- 1.**使用timespec结构，而不是timeval结构**。timespec的第二个成员是纳秒级，2而timeval的第二个成员是微妙级

```c
struct timespec {
    time_t tv_sec; //秒，这里也有区别，timeval中tv_sec为long类型
    long tv_nsec; //纳秒
}
```

- 2.**增加了第6个参数：一个指向信号掩码的指针**。该参数运行程序先禁止递交某些信号，再测试由这些当前被禁止信号的信号处理函数设置的全局变量，然后调用pselect，告诉它重新设置信号掩码（调用pselect时，根据sigmask代替进程的信号掩码，当pselect返回时，进程的信号 掩码又被重置为调用pselect之前的值）

## 6.10 poll函数

```c
#include <poll.h>

/*
** @fdarray:指向pollfd结构数组第一个元素的指针，pollfd用于指定测试某个给定描述符fd的条件
** @nfds：fdarray数组中元素的个数
** @timeout：设置poll函数返回前等待多长时间
**                    1. INFTIM（一个负值）：永远等待下去
**                    2. 0：立即返回，不阻塞进程
**                    3. >0：等待指定数目的毫秒数
** @返回值：
**       1. 正整数：所有就绪描述符的数目
**       2. 0：超时
**       3. -1：出错
*/
int poll(struct pollfd *fdarray, unsigned int nfds, int timeout);

/*
** events、revents不使用值-结果参数，可以避免每次调用poll函数前都要重新初始化
** 如果不再关心某个特定描述符，可以把与之对应的pollfd结构的fd成员设置成一个负值
** poll函数将忽略这样的pollfd结构的events成员，返回时将其revents成员的值置为0
*/
struct pollfd{
    int fd;         //需要测试的描述符
    short events;   //该描述符上测试的事件
    short revents;  //该描述符上发生的事件
};
```

**events与revents的值**

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_6_23_poll%E5%87%BD%E6%95%B0%E7%9A%84%E8%BE%93%E5%85%A5events%E5%92%8C%E8%BF%94%E5%9B%9Erevents.png)

- 第一部分是处理输入的4个常值
- 第二部分是处理输出的3个常值
- 第三部分是处理错误的3个常值

对于**普通数据**、**优先级带数据**、**高优先级数据**、**错误**的解释

- 所有正规TCP数据和所有UDP数据都被认为是**普通数据**
- TCP连接读半部关闭时（如收到一个来自对端的FIN），被认为是**普通数据**，随后读操作返回0
- TCP连接存在错误既可认为是**普通数据**，也可认为是**错误**。随后的读操作返回-1，并设置error（可用于处理诸如接收到RST或发生超时等条件）
- TCP的带外数据被认为是**优先级带数据**
- 在监听套接字上有新连接可用，既可认为是**普通数据**也可认为是**优先级带数据**
- 非阻塞式connect的完成被认为是使用套接字可写

**poll的优缺点**

优点：

- **没有最大监视描述符数量的限制**：分配一个pollfd结构的数组并把该数组中元素的数目通知内核成了调用者的责任。内核不再需要知道类似fd_set的固定大小的数据类型

缺点：

- 和select一样，调用返回后需要**轮询所有描述符来获取已经就绪的描述符**
- **用户态和内核态传递描述符结构时copy开销大**

## 6.11 TCP回射服务器程序（poll修订版）

使用poll代替select重写6.8节中的TCP回射服务器程序

在6.8的select版本中，必须分配一个client数组和一个rset描述符集

在本版本中只需要分配一个pollfd结构的数组来维护客户信息

```c
// 源码：tcpcliserv/tcpservpoll01.c
/* include fig01 */
#include	"unp.h"
#include	<limits.h>		/* for OPEN_MAX */

int
main(int argc, char **argv)
{
	//含有client数组当前正在使用的最大下标
	int					i, maxi, listenfd, connfd, sockfd;
	int					nready;
	ssize_t				n;
	char				buf[MAXLINE];
	socklen_t			clilen;
	struct pollfd		client[OPEN_MAX];
	struct sockaddr_in	cliaddr, servaddr;

	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);
	//client数组的一个项用于监听套接字
	client[0].fd = listenfd;
	//给监听套接字设为POLLRDNORM事件
	client[0].events = POLLRDNORM;
	//其余各项的描述符成员设为-1，表示当前不关心，poll函数将忽略这样的pollfd结构
	for (i = 1; i < OPEN_MAX; i++)
		client[i].fd = -1;		/* -1 indicates available entry */
	maxi = 0;					/* max index into client[] array */
/* end fig01 */

/* include fig02 */
	for ( ; ; ) {
		nready = Poll(client, maxi+1, INFTIM);

		if (client[0].revents & POLLRDNORM) {	/* new client connection */
			clilen = sizeof(cliaddr);
			connfd = Accept(listenfd, (SA *) &cliaddr, &clilen);
#ifdef	NOTDEF
			printf("new client: %s\n", Sock_ntop((SA *) &cliaddr, clilen));
#endif

			for (i = 1; i < OPEN_MAX; i++)
				if (client[i].fd < 0) {
					client[i].fd = connfd;	/* save descriptor */
					break;
				}
			if (i == OPEN_MAX)
				err_quit("too many clients");

			client[i].events = POLLRDNORM;
			if (i > maxi)
				maxi = i;				/* max index in client[] array */

			if (--nready <= 0)
				continue;				/* no more readable descriptors */
		}

		for (i = 1; i <= maxi; i++) {	/* check all clients for data */
			if ( (sockfd = client[i].fd) < 0)
				continue;
			//检查POLLERR的原因：
			//有些实现在一个连接上接收到RST时返回的是POLLERR事件
			//有些实现返回的却是POLLRDNORM事件
			if (client[i].revents & (POLLRDNORM | POLLERR)) {
				if ( (n = read(sockfd, buf, MAXLINE)) < 0) {
					if (errno == ECONNRESET) {
							/*4connection reset by client */
#ifdef	NOTDEF
						printf("client[%d] aborted connection\n", i);
#endif
						Close(sockfd);
						client[i].fd = -1;
					} else
						err_sys("read error");
				} else if (n == 0) {
						/*4connection closed by client */
#ifdef	NOTDEF
					printf("client[%d] closed connection\n", i);
#endif
					Close(sockfd);
					client[i].fd = -1;
				} else
					Writen(sockfd, buf, n);

				if (--nready <= 0)
					break;				/* no more readable descriptors */
			}
		}
	}
}
/* end fig02 */
```

## 6.12 epoll

[UNIX网络编程卷1---arking](https://github.com/arkingc/note/blob/master/计算机网络/UNIX网络编程卷1.md#4epoll)

# 第7章 套接字选项

## 7.1 概述

获取和设置影响套接字的选项的方法

- 1.getsockopt和setsockopt
- 2.fcntl函数：把套接字设置为阻塞式I/O、信号驱动式I/O以及设置套接字属主的POSIX的方法
- 3.ioctl函数

## 7.2 getsockopt和setsockopt

这两个函数仅用于套接字

```c
#include <sys/socket.h>

int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);

int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
```

- sockfd：必须指向一个打开的套接字描述符
- level：指定系统中解释选项的代码
  - 通用套接字代码（`SOL_SOCKET`）
  - 某个特定协议的代码（如IPv4（`IPPROTO_IP`），IPv6、TCP（`IPPROTO_TCP`）或SCTP）
- optval：指向某个变量的指针
  - setsockopt从*optval中取得选项待设置的新值
  - getsockopt把已获取的选项当前值放到*optval中
- optlen：指定*optval的大小
  - setsockopt：值参数
  - getsockopt：值-结果参数
- 返回值： 成功返回0，错误返回-1

对下图的**解释**：

- 1.表中“数据类型”列出了指针optval必须指向的数据类型，后跟花括号的表示一个结构，如`linger{}`表示`struct linger`

- 2.套接字选项粗分为两大基本类型：

  - **标志选项**：启用或禁止某个特性的二元选项，图中“标志”有黑圆点的，此时
    - *optval：一个整数，0表示被禁止，非0表示被启用
  - **值选项**：取回并返回我们可以设置或检查的特定值的选项，图中“标志”没有黑圆点的，此时
    - *optval：用于在用户进程与系统之间传递所指定数据类型的值

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_1_%E5%A5%97%E6%8E%A5%E5%AD%97%E5%B1%82%E5%92%8CIP%E5%B1%82%E7%9A%84%E5%A5%97%E6%8E%A5%E5%AD%97%E9%80%89%E9%A1%B9%E6%B1%87%E6%80%BB.png)

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_2_%E4%BC%A0%E8%BE%93%E5%B1%82%E7%9A%84%E5%A5%97%E6%8E%A5%E5%AD%97%E9%80%89%E9%A1%B9%E6%B1%87%E6%80%BB.png)

## 7.4 套接字状态

以下的套接字选项由TCP已连接套接字从监听套接字继承而来

- `SO_DEBUG`（开启调试跟踪）
- `SO_DONTROUTE`（绕过外出路由表查询）
- `SO_KEEPLIVE`（周期性测试连接是否存活）
- `SO_LINGER`（若有数据待发送则延迟关闭）
- `SO_OOBINLINE`（让接收到的带外数据继续再线留存）
- `SO_RCVBUF`（接收缓冲区大小）
- `SO_RCVLOWAT`（接收缓冲区低水位标记）
- `SO_SNDBUF`（发送缓冲区大小）
- `SO_SNDLOWAT`（发送缓冲区低水位标记）
- `TCP_MAXSEG`（TCP最大分节大小）
- `TCP_NODELAY`（禁止Nagle算法）

## 7.5 通用套接字选项

这些选项是协议无关的，不过其中有些选项只能应用到某些特定类型的套接字中。如称`SO_BROADCAST`套接字选项是“通用”的，但它只能用于数据报套接字

### 7.5.1 SO_BROADCAST套接字选项

**限制**：只有**数据报**套接字支持广播，并且必须是支持广播消息的网络上（如以太网）

**作用**：开启或禁止进程发送广播消息的能力

由于应用进程在发送广播数据报之前必须设置本套接字选项，因此它能够有效地防止一个进程在其应用程序根本没有设计成可广播时就发送广播数据报

如果发送数据报的目的地址为广播地址且本套接字选项没有设置，则返回EACCES错误

### 7.5.2 SO_DEBUG套接字选项

  **限制**：仅由TCP支持

  **作用**：当给一个TCP套接字开启本选项时，内核将为TCP在该套接字发送和接收的所有分组保留详细跟踪信息。这些信息保留在内核的某个唤醒缓冲区中，并可使用trpt程序进程检查

### 7.5.3 SO_DONTROUTE套接字选项

**作用**：规定外出的分组将绕过底层协议的正常路由机制

**举例**：如果开启本选项，IPv4情况下外出分组将被定向到适当的本地接口（由目的地址的网络和子网部分确定）。如果这样的本地接口无法由目的地址确定（如目的主机不在一个点对点链路的另一端，也不在一个共享的网络上），则返回ENETUNREACH错误

**相同效果的做法**：给函数send、sendto、sendmsg使用MSG_DONTROUTE标志，也能在个别的数据报上取得相同效果

**应用**：路由守护进程（routed和gated）使用本选项绕过路由表（路由表不正确的情况下），以强制将分组从特定接口送出

### 7.5.4 SO_ERROR套接字选项

**背景**:当一个套接字上发生错误时，源自Berkeley的内核中的协议模块将该套接字的名为so_error的变量设为标准的Unix Exxx值中的一个，称为该套接字的**待处理错误**

**内核通知进程该错误的方法**：

- 1.如果进程阻塞在该套接字的select调用上，那么无论是检查该套接字的可读条件还是可写条件，select均返回并设置其中一个或两个条件为“就绪”
- 2.如果进程使用信号驱动I/O模型，内核给进程或进程组产生一个SIGIO信号

**进程获取该错误的方法**：使用`getsockopt + SO_ERROR`获取，其中*valopt就是该套接字的待处理错误。获取之后，so_error由内核复位为0

**read和write发现该错误的过程**：

- read：
  - 无数据可读时：当进程调用read且没有数据返回时，如果`so_error`为非0，则read返回-1且**errno**被设置为`so_error`的值，随后so_error被复位为0
  - 有数据可读时：如果该套接字上有数据在排队等待读取，read返回数据而不是错误条件
- write：
  - 如果进程在调用write时so_error为非0，write返回-1，且**errno**被设置为`so_error`的值，随后`so_error`被复位为0

### 7.5.5 SO_KEEPLIVE套接字选项

**功能**：检测对端主机是否崩溃或变得不可达

给一个TCP套接字设置**保持存活选项**后，如果2个小时内在该套接字的任何一方上没有数据交换，TCP就自动给对端发送一个**保持存活探测分节**，此时对端必须响应，并可能产生3种情况：

- 1.**正常**：对端以期望的ACK响应，应用进程得不到通知
- 2.**对端已经崩溃且已重启**：对端以RST响应，套接字的待处理错误被置为ECONNRESET，套接字本身被关闭
- 3.**无响应**：对端对**保持存活探测分节**没有任何响应，Berkeley的TCP将另外发送8个探测分节，两两相隔75秒，试图得到响应。在第一个探测分节后11分15秒内若没有任何响应则放弃
  - 没有任何响应：套接字的待处理错误设为ETIMEDOUT，套接字本身被关闭
  - 收到ICMP错误，返回相应错误，套接字本身被关闭
    - 如，ICMP错误为“host unreachable”（主机不可达），说明对端没有崩溃，只是不可达，待处理错误设为EHOSTUNREACH

**修改探测定时**：通常为2小时。如果修改该参数,则会影响到主机上所有开启本选项的套接字。因为是基于整个内核维护这些时间参数的，而不是基于每个套接字维护

**谁启用**：一般由服务器使用，因为它们花大部分时间阻塞在等待穿越TCP连接的输入上，如果客户主机连接掉线、电源掉电、系统崩溃，服务器进程将永远不知道，并永远等待，这种状态称为**半开连接**。保持存活选项将检测出这些搬开连接并终止它们

**替代方法**：应用层超时，由应用层进程本身完成

下图，展示了一个TCP连接的另一端发生某些事情时我们可以采用的各种检测方法的汇总

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_6_%E6%A3%80%E6%B5%8B%E5%90%84%E7%A7%8DTCP%E6%9D%A1%E4%BB%B6%E7%9A%84%E6%96%B9%E6%B3%95.png)

### 7.5.6 SO_LINGER套接字选项

**该选项指定close函数对面向连接的协议如何操作**。默认操作是close函数立即返回，但是如果有数据残留在套接字发送缓冲区中，系统将试着把这些数据发送给对端。SO_LINGER套接字选项使得我们可以改变这个默认设置 

选项要求在用户进程与内核间传递如下结构：

```c
  //定义在头文件<sys/socket.h>
  struct linger{
  	int l_onoff;   /* 0=off（关闭）, nozero=on（开启） */
  	int l_linger;  /* 延滞时间，POSIX指定单位为秒 */
  };
```

有以下3种情形：

- 1.**l_onoff为0**：此时l_linger值会被忽略。关闭该选项，默认设置生效，即close立即返回 （**图7.7**） 

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_7_close%E7%9A%84%E9%BB%98%E8%AE%A4%E6%93%8D%E4%BD%9C%EF%BC%9A%E7%AB%8B%E5%8D%B3%E8%BF%94%E5%9B%9E.png)

- 2.**l_onoff非0，且l_linger为0**：TCP将终止连接，丢弃保留在套接字发送缓冲区中的任何数据，并发送一个RST给对端，而没有通常的四分组连接终止序列，这么一来**避免了TIME_WAIT状态**，然而存在以下可能性：在2MSL秒内创建该连接的另一个化身，导致来自刚被终止的连接上的旧的重复分节被不正确地传送到新的化身上 
- 3.**l_onoff非0，且l_linger非0**：当套接字关闭时，内核将拖延一段时间。如果在套接字发送缓冲区中仍残留有数据，那么进程将投入睡眠，直到：
  - (a)所有数据都已经发送完且均被对方确认；
  - (b)延滞时间到；（如果套接字被设置为非阻塞，那么将不等待close完成，即使延滞时间非0也是如此） 

当使用SO_LINGER选项的这个特性时，应用进程检查**close的返回值**是非常重要的，因为如果在数据发送完并被确认前延滞时间到的话，close将返回-1，errno设为EWOULDBLOCK错误，且套接字发送缓冲区中的任何残留数据都被丢弃。这种组合可能存在下列几种情况： 

- **延滞时间合适**：close直到数据和FIN已被服务器主机的TCP确认后才返回（**图7.8**） 
  - 问题：在服务器应用进程读剩余数据之前，服务器主机可能崩溃，并且客户应用进程永远不会知道。因此，close成功返回只是告诉我们先前发送的数据和FIN已由对端TCP确认，而不能告诉我们对端应用进程是否已读取数据 
- **延滞时间偏低**：在接收到服务器主机的TCP确认前close返回 （**图7.9**） 

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_8_SO_LINGER%E5%A5%97%E6%8E%A5%E5%AD%97%E9%80%89%E9%A1%B9%E4%B8%94l_linger%E4%B8%BA%E6%AD%A3%E5%80%BC%E6%97%B6%E7%9A%84close.png)

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_9_%E8%AE%BE%E7%BD%AESO_LINGER%E5%A5%97%E6%8E%A5%E5%AD%97%E9%80%89%E9%A1%B9%E4%B8%94l_linger%E4%B8%BA%E5%81%8F%E5%B0%8F%E6%AD%A3%E5%80%BC%E6%97%B6%E7%9A%84close.png)

**一个基本原则**：设置`SO_LINGER`套接字选项后，close的成果返回只是告诉我们先前发送的数据和FIN已经由对端TCP确认，而不能告诉我们对端应用进程是否已经读取数据

**知道对端已经读取数据的方法**：

- 1.使用shutdown，且第二个参数为SHUT_WR，后跟一个read调用--->read直到收到对端的FIN才返回0（**图7.10**）

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_10_%E7%94%A8shutdown%E6%9D%A5%E8%8E%B7%E7%9F%A5%E5%AF%B9%E6%96%B9%E5%B7%B2%E6%8E%A5%E6%94%B6%E6%95%B0%E6%8D%AE.png)

- 2.应用级确认，也称为“应用ACK"(**图7.11**)

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_11_%E5%BA%94%E7%94%A8ACK.png)

**在客户端**：向服务器发送数据后调用read来获取1个字节的数据

```c
  char ack;
  Write(sockfd, data, nbytes); // 从客户到服务器的数据
  n = Read(sockfd, &ack, 1); // 等待应用ACK
```

**服务器**：读取客户的数据后发回1个字节的应用级ACK

```c
  nbytes = Read(sockfd, buff, sizeof(buff)); // 来自客户的数据
  Write(sockfd,"",1); //服务器发送给客户的ACK
```

  

下图汇总了对**shutdown**的两种可能调用和对**close**的三种可能调用以及它们对TCP套接字的影响：

  ![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_12_shutdown%E5%92%8CSO_LINGER%E5%90%84%E7%A7%8D%E6%83%85%E5%86%B5%E7%9A%84%E6%80%BB%E7%BB%93.png)

### 7.5.8 SO_RCVBUF和SO_RCVSNDBUF套接字选项

**作用**：每个套接字都有一个发送缓冲区和一个接收缓冲区。这两个套接字选项允许我们改变这两个缓冲区的默认大小 

**TCP流量控制**：对于TCP来说，套接字接收缓冲区中可用空间的大小限定了TCP通告对端的窗口大小。TCP套接字接收缓冲区不可能溢出，因为不允许对端发出超过本端所通告窗口大小的数据。如果对端无视窗口大小而发出了超过该窗口大小的数据，本端TCP将丢弃它们 

**UDP无流量控制**：对于UDP来说，当接收到的数据报装不进套接字接收缓冲区时，该数据报就被丢弃。UDP是没有流量控制的：较快的发送端可以很容易地淹没较慢的接收端，导致接收端的UDP丢弃数据报，事实上较快的发送端甚至可以淹没本机的网络接口，导致数据报被本机丢弃 

**TCP窗口规模的建立**：TCP的窗口规模选项是在建立连接时用SYN分节与对端互换得到的，因此SO_RCVBUF的设置要在建立连接之前完成：

- 对于**客户** 
  - SO_RCVBUF选项必须在调用connect之前设置
- 对于**服务器**
  - SO_RCVBUF选项必须在调用listen之前给监听套接字设置 
  - 给已连接套接字设置该选项对于可能存在的窗口规模选项没有任何影响，因为accept直到TCP的三路握手完成才会创建并返回已连接套接字。这就是必须给监听套接字设置该选项的原因。（套接字缓冲区的大小总是由新创建的已连接套接字从监听套接字继承而来） 

**TCP缓冲区的大小**：

- 至少为MSS的4倍。依据是TCP快速恢复算法（3个重复的确认来检测某个分节是否丢失）
- 应该是响应连接的MSS值的偶数倍
- 如果套接字缓冲区小于**带宽-延迟积**，连接管道将不会处于满状态
  - 带宽-延迟积：带宽和RTT相乘

### 7.5.9 SO_RCVLOWAT和SO_SNDLOWAT套接字选项

每个套接字还有一个**收低水位标记**和一个**发送低水位标记**。它们由select函数使用 

**接收低水位标记** :让select返回“可读”时套接字接收缓冲区中所需的数据量（对于TCP、UDP、SCTP套接字，其默认值是1） 

**发送低水位标记**：让select返回“可写”时套接字发送缓冲区中所需的可用空间 （对于TCP套接字，其默认值通常是2048；UDP也使用发送低水位标记，然而由于UDP并不为应用程序传递给它的数据报保留副本，因此UDP套接字的发送缓冲区中可用空间的字节数从不改变，只要一个UDP套接字的发送缓冲区大小大于该套接字的低水位标记，该UDP套接字就总是可写的。UDP并没有发送缓冲区，只有发送缓冲区大小这个属性） 

### 7.5.10 SO_RCVTIMEO和SO_SNDTIMEO套接字选项

这两个选项允许我们给套接字的接收和发送设置一个超时值。访问它们的getsockopt和setsockopt函数的参数指向timeval结构的指针，与select所用参数相同

```c
struct timeval {
    long tv_sec; //秒数
    long tv_usec; //微秒
};
```

**接收超时**影响5个输入函数：

- read
- readv
- recv
- recvfrom
- recvmsg

**发送超时**影响5个输出函数：

- write
- writev
- send
- sendto
- sendmsg

### 7.5.11 SO_REUSEADDR和SO_REUSEPOPT套接字

**SO_REUSEADDR**套接字选项的4个功能

> **1.允许启动一个监听服务器并捆绑其众所周知端口，即使以前建立的将该端口用作它们的本地端口的连接仍然存在**，这个条件通常是这样产生的：

- a）启动一个监听服务器
- b）连接请求到达，派生一个子进程来处理这个客户
- c）监听服务器终止，但子进程继续为现有连接上的客户提供服务
- d）重启监听服务

**解释**：默认情况下，当监听服务器在步骤d通过调用socket、bind和listen重新启动时，由于它试图绑定一个现有连接（即正由早先派生的那个子进程处理着的连接）上的端口，从而bind调用会失败。但如果该服务器在socket和bind两个调用之间设置了SO_REUSEADDR套接字选项，那么bind成功。

**建议**：所有TCP服务器都应该指定本套接字选项，以允许服务器在这种情况下被重新启动

> **2.允许在同一端口上启动同一服务器的多个实例（多个进程），只要每个实例捆绑一个不同的本地IP地址即可** 

**举例**：三个服务器实例，一个使用通配地址、另两个使用指定地址

**建议**：执行通配地址的服务器进程最后一个启动

> **3.允许单个进程捆绑同一端口到多个套接字上，只要每次捆绑指定不同的本地IP地址即可** 

**举例**：

- 1.要求知道客户请求的目的IP地址的UDP服务器
- 2.希望在一个多目的主机的若干个（非全部）本地地址上服务连接的TCP服务器

> **4.允许完全重复的捆绑：当一个IP地址和端口已捆绑到某个套接字上时，如果传输协议支持，同样的IP地址和端口还可以捆绑到到另一个套接字上**

**举例**：一般来说该特性仅支持UDP套接字，用于多播

**所用规则**：如果数据报的目的地址时一个广播地址或多播地址，那就给每个匹配的套接字递送一个该数据报的副本。如果是单播地址，且有多个套接字匹配该数据报，那么该选择由哪个套接字接收它取决于实现

------

**SO_REUSEPOPT**的功能

- 1.本选项允许完全重复的绑定，不过只有在想要捆绑同一IP地址和端口的每个套接字都指定了套接字选项才行
- 2.如果被捆绑的IP地址时一个多播地址，那么`SO_REUSEADDR`和`SO_REUSEPOPT`被认为是等效的

**缺点**：并非所有系统都支持，不支持时，可改用SO_REUSEADDR

------

**总结**：

- 1.在所有TCP服务器程序中，在调用bind之前设置SO_REUSEADDR套接字选项
- 2.当编写一个可在同一时刻在同一主机上运行多次的多播应用时，设置SO_REUSEADDR套接字选项，并将所有参加多播组的地址作为本地IP地址捆绑

### 7.5.12 SO_TYPE套接字选项

**功能**：返回套接字的类型，返回的整数值时一个诸如`SOCK_STREAM`或`SOCK_DGRAM`之类的值

**场景**：通常由启动时继承了套接字的进程使用

## 7.6 IPv4套接字选项

这些套接字选项由IPv4处理，它们的级别为IPPROTO_IP

### 7.6.2 IP_OPTIONS套接字选项

**作用**：允许我们在IPv4首部中设置IP选项

将在27.3节讨论

### 7.6.3 IP_RECVDSTADDR套接字选项

**作用**：导致所收到的UDP数据报的目的IP地址由recvmsg函数作为辅助数据返回

将在22.2节给出一个例子

### 7.6.4 IP_RECVIF套接字选项

**作用**：导致所收到的UDP数据报的接收接口索引由recvmsg函数作为辅助数据返回

将在22.2节给出一个例子

### 7.6.5 IP_TOS套接字选项

**作用**：允许我们为TCP、UDP或SCTP套接字设置IP首部中的服务类型字段（该字段包含DSCP和ECN子字段）

**给本选项调用getsockopt**：

- 1.用于放入外出IP数据报首部的DSCP和ECN字段中的EOS当前值（默认为0）将返回
- 2.没有办法从接收到的IP数据报中取得该值

### 7.6.6 IP_TTL套接字选项

**作用**：设置或获取系统用在从某个给定套接字发送的单播分组上的默认TTL值（多播TTL值使用IP_MULTICAST_TTL套接字选项）

**默认值**：

- 1.TCP、UDP：64
- 2.原始套接字：255

**给本选项调用getsockopt**：

- 1.返回系统将用于外出数据报的字段的默认值
- 2.没有办法从接收到的IP数据报中取得该值

## 7.9 TCP套接字选项

TCP有两个套接字选项， 它们的级别为IPPROTO_TCP 

### 7.9.1 TCP_MAXSEG套接字选项

**作用**：允许我们获取或设置TCP连接的最大分节大小(MSS)

返回值是我们的TCP可以发送给对端的最大数据量，它通常是由对端使用SYN分节通告的MSS，除非我们的TCP选择使用一个比对端通告的MSS小些的值。如果该值在相应套接字的连接建立之前取得，那么返回值是未从对端收到MSS选项的情况下所用的**默认值**。如果用上如“时间戳选项”的话， 那么实际用于连接中的最大分节大小可能小于该套接字选项的返回值，因为时间戳选项在每个分节中要占用12字节的TCP选项容量 

如果TCP支持路径MTU发现功能，那么它将发送的每个分节的最大数据量还可能在连接存活期内改变。如果到对端的路径发送变动，该值就会有所调整 

### 7.9.2 TCP_NODELAY套接字选项

**作用**：开启该选项将禁止TCP的Nagle算法，默认情况下是启动的

**Nagle算法的目的**：减少广域网上小分组的数目，防止一个连接在任何时刻有多个小分组待确认 

**Nagle算法指出**：如果某个给定连接上有**待确认数据**，那么原本应该作为用户写操作在该连接上立即发送相应小分组的行为就不会发生，直到现有数据被确认为止

**“小分组”的定义**： 小于MSS的任何分组

Rlogin和Telnet的客户端是两个常见的小分组产生进程。考虑一个例子，在Rlogin或Telnet的客户端键入6个字符的串“hello!”，每个字符间间隔正好是250ms。到服务器端的RTT为600ms，而且服务器立即发回每个字符的回显。假设对客户端字符的ACK是和字符回显一同发回给客户端的，并且忽略客户端发送的对服务器端回显的ACK，下图展示了禁止Nagle算法（**图7.14**）和开启时（**图7.15**）的情况，在开启时，会感觉到明显的延迟： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_7_14_Pic_7_15_Nagle%E7%AE%97%E6%B3%95.png)

**ACK延滞算法**：Nagle算法常常与另一个TCP算法联合使用：**ACK延滞算法**，该算法使得TCP在接收到数据后不立即发送ACK，而是等待一小段时间（典型值为50ms~200ms），然后才发送ACK。TCP期待在这一小段时间内自身有数据发送回对端，被延滞的ACK就可以由这些数据捎带，从而省掉一个TCP分节

**可行场景**： 对于Rlogin和Telnet客户来说通常可行，因为他们的服务器一般都回显客户发送来的每个字符，这样对客户端字符的ACK完全可以在服务器对该字符的回显中捎带返回 

**不可行场景**：

- 1.于服务器不在相反方向产生数据以便捎带ACK的客户来说，ACK延滞算法存在问题 。客户可能觉察到明显的延迟（定时器到时才发送ACK） 
- 2.客户是以小片数据向服务器发送单个逻辑请求的客户。如发送一个400字节的请求（4字节请求类型+396字节请求数据），分别调用两次write
  - 解决方法：
    - 1.使用writev而不是两次调用write，只产生一个TCP分节
    - 2.将4字节和396字节先复制到单个缓冲区中，再对该缓冲区调用一次write
    - 3.设置TCP_NODELAY套接字选项，继续调用write两次（不推荐）

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

# 第8章 基本UDP套接字编程

## 8.1 概述

使用UDP编写的一些常见的应用程序有：DNS（域名系统）、NFS（网络文件系统）、SNMP（简单网络管理协议）

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_1_UDP%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%A8%8B%E5%BA%8F%E6%89%80%E7%94%A8%E7%9A%84%E5%A5%97%E6%8E%A5%E5%AD%97%E5%87%BD%E6%95%B0.png)

## 8.2 recvfrom和sendto函数

```c++
#include <sys/socket.h>

/*
** 共同参数
** @sockfd:描述符
** @buff:指向读入或写出缓冲区的指针
** @nbytes:读写字节数
** @flags：将在14章讨论，目前都设为0
*/

/*
** 作用：
** @from:指向一个将由该函数在返回时填写数据报发送者的协议地址的套接字地址结构
** @addrlen:from所指向的套接字地址结构的大小，注意：值-结果参数
** @返回值：成功：所读数据的长度；出错：-1
*/
sszie_t recvfrom(int sockfd, void *buff, size_t nbytes, int flags, struct sockaddr *from, socklen_t *addrlen);

/*
** 作用：
** @to:指向一个含有数据报接收者的协议地址（IP地址和端口号）的套接字地址结构
** @addrlen:to所指向的套接字地址结构的大小，注意：整数值
** @返回值：成功：所写数据的长度；出错：-1
*/
ssize_t sentto(int sockfd, const void *buff, size_t nbytes, int flags, const struct sockaddr *to, socklen_t addrlen);
```

- recvfrom的最后两个参数（from、addrlen）类似与accept（cliaddr、addrlen）的最后两个参数
  - UDP情况下：返回时其中套接字地址结构的内容告诉我们是谁发送了数据报
  - TCP情况下：谁发起了连接
- sendto的最后两个参数（to、addren）类似与connect（servaddr、addrlen）的最后两个参数
  - UDP情况下：调用时其中套接字地址结构被我们填入数据报将发往的协议地址
  - TCP情况下：与之建立连接的协议地址

**写0长度的数据报**

写一个长度为0的数据报是可行的，UDP情况下，会形成一个只包含一个IP首部（IPv4为20字节，IPv6为40字节）和一个8字节UDP首部而没有数据的IP数据报

**读0长度的数据报**

recvfrom返回值是0，它并不像TCP套接字read返回0表示对端已经关闭连接，因为UDP无连接，因此没有关闭连接之说。返回值为0说明读到的对端发送的“写0长度的数据报”

**from参数为空**

recvfrom的from参数可以是空指针，此时addrlen也必须为空，表示我们不关心数据报发送者的协议地址

**TCP使用这两个函数**

recvfrom和sendto可以用于TCP，但通常不会这么做

## 8.3 UDP回射服务器程序：main函数

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_2_%E4%BD%BF%E7%94%A8UDP%E7%9A%84%E7%AE%80%E5%8D%95%E5%9B%9E%E5%B0%84%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

```c++
// 源码： udpcliserv/udpserv01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_in	servaddr, cliaddr;

	//SOCK_DGRAM表示创建一个UDP套接字
	sockfd = Socket(AF_INET, SOCK_DGRAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);

	//#define SERV_PORT 9877
	servaddr.sin_port        = htons(SERV_PORT);

	Bind(sockfd, (SA *) &servaddr, sizeof(servaddr));

	//调用dg_echo函数来执行服务器的处理工作
	dg_echo(sockfd, (SA *) &cliaddr, sizeof(cliaddr));
}
```

## 8.4 UDP回射服务器程序：dg_echo函数

```c++
// 源码：lib/dg_echo.c

#include	"unp.h"

void
dg_echo(int sockfd, SA *pcliaddr, socklen_t clilen)
{
	int			n;
	socklen_t	len;
	char		mesg[MAXLINE];

	//简单的循环
	for ( ; ; ) {
		len = clilen;
		//使用recvfrom读入下一个到达服务器端口的数据报
		n = Recvfrom(sockfd, mesg, MAXLINE, 0, pcliaddr, &len);

		//把读到的数据报再发送回去
		Sendto(sockfd, mesg, n, 0, pcliaddr, len);
	}
}
```

**该函数的特点**

- 1.该函数永不终止，因为UDP是一个无连接的协议，它没有像TCP中EOF之类的东西
- 2.该函数提供的是一个迭代服务器。一般来说，TCP服务器是并发的，UDP服务器是迭代的

**接收缓冲**

对于本套接字，UDP层中隐含有排队发生。事实上每个UDP套接字都有一个接收缓冲区，到达该套接字的每个数据报都进入该套接字接收缓冲区。

- 每当调用recvfrom时，缓冲区中的下一个数据报以FIFO顺序返回给进程
- 如果有多个数据报到达该套接字，那么相继到达的数据报仅仅加到该套接字的接收缓冲区中（接收缓冲区大小是有限的，通过SO_RCVBUF套接字选项修改）

**TCP与UDP套接字对比**

- TCP套接字

服务器上有两个已连接套接字，每一个都有各自的套接字接收缓冲区

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_5_%E4%B8%A4%E4%B8%AA%E5%AE%A2%E6%88%B7%E7%9A%84TCP%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B0%8F%E7%BB%93.png)

- UDP套接字

只有一个服务器进程，仅有的单个套接字用于接收所有到达的数据报并发回所有的响应。该套接字只有一个接收缓冲区用来存放所有到达的数据报

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_6_%E4%B8%A4%E4%B8%AA%E5%AE%A2%E6%88%B7%E7%9A%84UDP%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%B0%8F%E7%BB%93.png)

## 8.5 UDP回射客户程序：main函数

```c++
// 源码： udpcliserv/udpcli01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_in	servaddr;

	if (argc != 2)
		err_quit("usage: udpcli <IPaddress>");

	//把服务器的IP地址和端口号填入一个IPv4的套接字地址结构
	//该结构将传递给dg_cli函数，以指明数据报发往何处
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	servaddr.sin_port = htons(SERV_PORT);
	Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);

	//创建一个UDP套接字
	sockfd = Socket(AF_INET, SOCK_DGRAM, 0);

	dg_cli(stdin, sockfd, (SA *) &servaddr, sizeof(servaddr));

	exit(0);
}
```

## 8.6 UDP回射客户程序：dg_cli函数

```c++
// 源码： lib/dg_cli.c

#include	"unp.h"

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int	n;
	char	sendline[MAXLINE], recvline[MAXLINE + 1];

	//使用fgets从标准输入读入一行文本行
	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		//使用sendto将该文本行发送给服务器
		Sendto(sockfd, sendline, strlen(sendline), 0, pservaddr, servlen);

		//使用recvfrom读回服务器的回射
		n = Recvfrom(sockfd, recvline, MAXLINE, 0, NULL, NULL);

		recvline[n] = 0;	/* null terminate */
		//使用fputs把回射的文本行显示到标准输出
		Fputs(recvline, stdout);
	}
}
```

**端口号**

该函数未请求内核给它的套接字指派一个临时端口。

对于临时端口的指派：

- 1.TCP：connect调用时指派
- 2.UDP：进程首次调用sendto时如果没有绑定一个本地端口，内核在此时为它选择一个临时端口

显示指定端口：UDP客户也可以显示调用bind指定端口，但很少这么做

**风险（待解决的问题）**

recvfrom指定的第五个和第六个参数（from、addrlen）是空指针，这告知内核我们不关心应答数据报由谁发送

这样做存在一个风险：任何进程不论是在与本客户进程相同的主机还是在不同的主机上，都可以向本客户的IP地址和端口号发送数据报，这些数据报被客户读入并认为是服务器的应答。该问题将在8.8节中解决

## 8.7 数据报的丢失

UDP客户/服务器例子是不可靠的

- 1.如果一个**客户数据报丢失**（例如，被客户主机与服务器主机之间的某个路由丢弃），客户将永远阻塞与dg_cli函数中的recvfrom调用，等待一个永远不会到达的服务器应答
- 2.如果客户数据报到达服务器，但**服务器应答丢失**，客户也将永远阻塞与recvfrom调用

解决方法：给客户的recvfrom设置一个超时（14.2节）。但该方法不是完整的解决方案，因为如果确实超时了，我们无法判定超时原因是数据报没到达服务器，还是服务器的应答没有回到客户（22.5节增加UDP客户/服务器可靠性）

## 8.8 验证收到的响应（dg_cli服务器IP验证版）

需要修改的地方：

- 1.把客户端的main函数改为使用标准回射服务器

```c++
//将客户main函数的
servaddr.sin_port = htons(SERV_PORT);
//改为
servaddr.sin_port = htnos(7);
```

- 2.修改dg_cli函数

```c++
// 源码： udpcliserv/dgcliaddr.c

#include	"unp.h"

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int				n;
	char			sendline[MAXLINE], recvline[MAXLINE + 1];
	socklen_t		len;
	struct sockaddr	*preply_addr;

	//调用malloc分配另一个套接字地址结构
	preply_addr = Malloc(servlen);

	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		Sendto(sockfd, sendline, strlen(sendline), 0, pservaddr, servlen);

		len = servlen;
		n = Recvfrom(sockfd, recvline, MAXLINE, 0, preply_addr, &len);
		//首先比较由recvfrom在值-结果参数中返回的长度
		//再用memcpy比较套接字地址结构本身
		if (len != servlen || memcmp(pservaddr, preply_addr, len) != 0) 		{
			printf("reply from %s (ignored)\n",
					Sock_ntop(preply_addr, len));
			continue;
		}

		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
}
```

**问题**

如果服务器运行在一个只有单个IP地址的主机上，该版本的客户工作正常

如果服务器主机是所宿的，该客户有可能失败

```shell
# 获取freebsd4服务器的ip地址
# macosx为客服主机名
macosx % host freebsd4
freebsd4.unpbook.com has address 172.24.37.94 # freebsd4有两个IP地址
freebsd4.unpbook.com has address 135.197.17.100

# 客户向freebsd4的135.197.17.100IP发送数据报
macosx % udpcli02 135.197.17.100
hello # 客户输入
reply from 172.24.37.94:7 (ignored) #由于数据报的源IP为服务器的另一个IP，因此被忽略
goodbye # 客户输入
reply from 172.24.37.94:7 (ignored) # 同上
```

**解决办法**

- 1.**客户端解决**：得到由recvfrom返回的ip地址后，客户通过DNS中查找服务器主机的名字来验证该主机的域名
- 2.**服务器解决**UDP服务器给服务器主机上配置的每个IP地址创建一个套接字，用bind捆绑每个IP地址到各自的套接字，然后再所有套接字上使用select（等待其中任何一个变得可读），再从可读的套接字给出应答，这样可以保证应答的源地址与请求的目的地址相同

## 8.9 服务器进程未运行

**情景**：不启动服务器的前提下启动客户，客户键入一行文本。那么什么也不会发生，客户永远阻塞于它的recvfrom调用，等待一个永远不出现的服务器应答

首先，在主机macosx上启动tcpdump，然后在同一主机上启动客户，指定主机freebsd4为服务器主机

```shell
#客户
macosx % udpcli01 172.24.37.94
hello world
```

tcpdump的输出

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_10_%E5%BD%93%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%BB%E6%9C%BA%E4%B8%8A%E6%9C%AA%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BF%9B%E7%A8%8B%E6%97%B6tcpdump%E7%9A%84%E8%BE%93%E5%87%BA.png)

从第4行看到，服务器主机响应的是一个“port unreachable”（端口不可达）ICMP消息，不过这个ICMP消息不返回给客户进程

该ICMP错误称为**异步错误**。该错误由sendto引起，但是sendto本身却成功返回

UDP中，sendto成功返回仅仅表示在接口输出队列中具有存放所形成IP数据报的空间。该ICMP错误直到后来才返回，这就是称其为**异步的原因**

**为什么不返回**：如果在同一UDP套接字上连续向不同IP发生3个数据报，并阻塞与recvfrom，其中只有1个IP无法到达，发送这3个数据报的客户需要知道引发错误的数据报的目的地址以区分究竟是哪一个数据报引发了错误。但是内核如何把信息返回给客户进程呢？recvfrom可以返回的信息仅有errno，它没有办法返回出错数据报的目的IP地址和目的UDP端口号，因此作出决定：仅在进程已经被其UDP套接字连接到恰恰一个对端后，这些异步错误才返回给进程

**基本规则**：对于一个UDP套接字，由它引发的异步错误却并不返回给它，除非它已连接（8.11节讨论如何给UDP套接字调用connect从而完成“连接”）。仅在进程已经被其UDP套接字连接到恰恰一个对端后，这些异步错误才返回给进程

## 8.10 UDP程序例子小结

> 客户角度

图中圆点表示发送UDP数据报时必须指定或选择的四个值

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_11_%E4%BB%8E%E5%AE%A2%E6%88%B7%E8%A7%92%E5%BA%A6%E6%80%BB%E7%BB%93UDP%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

**必须指定的**：服务器的IP地址和端口号

**可选择的**：客户的IP地址和端口号（可由内核自动选择，也可以调用bind指定）

- 内核自动选择**端口号**：第一次调用sendto时一次性选择，**不能改变**
- 内核自动选择**IP地址**：可以随客户发送的每个UDP数据报而变动（假设客户没有捆绑一个具体的IP地址到其套接字上）。**原因**：如图，如果客户主机是所宿的，客户有可能在两个目的地址之间交替选择，其中一个由左边的数据链路外出，另一个由右边的数据链路外出，由内核基于外出数据链路选择的客户IP地址将随每个数据报而改变

**问题**：如果客户绑定到一个IP地址到其套接字上，但内核决定外出数据报必须从另一个链路发出，会发生什么？

- 答：IP数据报将包含一个不同于外出链路IP地址的源IP地址（习题8.6）

> 服务器角度

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_12_%E4%BB%8E%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%A7%92%E5%BA%A6%E6%80%BB%E7%BB%93UDP%E5%AE%A2%E6%88%B7%E6%9C%8D%E5%8A%A1%E5%99%A8.png)

服务器可能想从到达的IP数据报上获取至少四条信息：源IP地址、目的IP地址、源端口号、目的端口号，下图显示了TCP服务器和UDP服务器返回这些信息的函数调用

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_13_%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%8F%AF%E4%BB%8E%E5%88%B0%E8%BE%BE%E7%9A%84IP%E6%95%B0%E6%8D%AE%E6%8A%A5%E4%B8%AD%E8%8E%B7%E5%8F%96%E7%9A%84%E4%BF%A1%E6%81%AF.png)

UDP套接字的**目的IP地址**只能通过IPv4设置`IP_RECVDSTADDR`套接字选项（或IPv6设置`IPV6_PKTINFO`套接字选项）然后调用**recvmsg**。由于UDP是无连接的，因此目的IP地址可随发送到服务器的每个数据报而改变

## 8.11 UDP的connect函数

**给UDP套接字调用connect**：不给对端主机发送任何信息，没有三次握手过程，它完全是一个本地操作，内核只是检查是否存在立即可知的错误（如一个显然不可达的目的地），记录对端的IP地址和端口号（取自传递给connect的套接字地址结构），然后connect立即返回到调用进程。如果在一个未绑定到端口号的UDP套接字上调用connect同时也给该套接字指派一个临时端口

**未连接UDP套接字**：新创建UDP套接字默认状态

**已连接UDP套接字**：对UDP套接字调用connect的结果

已连接套接字与未连接套接字相比，有**三个变化**：

- 1.不再给输出操作指定目的IP地址和端口号，不使用sendto（也可以使用，但不能指定目的地址，即to、addrlen参数必须为空和0），而**改用write或send**。写到已连接UDP套接字上的任何内容都自动发送到由connect指定的协议地址
- 2.不使用recvfrom获取数据报的发送者，而**改用read、recv或recvmsg**。在一个已连接UDP套接字上，由内核为输入操作返回的数据报只有那些来自connect所指定协议地址的数据报。目的地位这个已连接套接字的本地协议地址，发源地不是该套接字早先conncect到的协议地址的数据报，不会投递到给套接字。已连接套接字仅能与一个对端交换数据报
- 3.由已连接UDP套接字引发的异步错误会返回给它们所在的进程，而未连接UDP套接字不接收任何异步错误

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_8_14_TCP%E5%92%8CUDP%E5%A5%97%E6%8E%A5%E5%AD%97%EF%BC%9A%E5%8F%AF%E6%8C%87%E5%AE%9A%E7%9B%AE%E7%9A%84%E5%8D%8F%E8%AE%AE%E5%9C%B0%E5%9D%80%E5%90%97.png)

**不对应的数据报如何处理**：来自任何其他IP地址或端口的数据报，不投递给不与其对应的已连接UDP套接字，可能投递给同一个主机上的其他某个UDP套接字，如果没有匹配的其他套接字，UDP将丢弃它们，并生成相应的ICMP端口不可达错误

### 8.11.1 给一个UDP套接字多次调用connect

一个拥有**已连接UDP套接字**的进程，可出于下列2个目的再次调用connect： 

- 1.**指定新的IP地址和端口号** 
  - 对于TCP，要再次调用connect必须先close套接字再重新调用socket创建套接字描述符 
- 2.**断开套接字**： 调用connect时，把套接字地址结构的地址族成员设置为AF_UNSPEC 
  - 对于IPv4为sin_family，对于IPv6为sin6_family
  - 可能回返回一个EAFNOSUPPORT错误，不过没关系，使套接字断开连接的是在已连接UDP套接字上调用connect的进程

### 8.11.2 性能

当应用进程要给同一目的地址发送多个数据报时，使用连接套接字可以获得更高的效率 ，因为：

- 1.当应用进程在一个未连接的UDP套接字上调用sendto时，源自Berkeley的内核暂时连接该套接字，发送数据报，然后断开该连接
- 2.减少搜索路由表的次数
  - 第一次临时连接需为目的IP地址搜索路由表并高速缓存这条信息
  - 第二次临时连接注意到目的地址等于已告诉缓存的路由表信息的目的地，于是就不必再次查找路由表
- 3.已连接UDP套接字只复制一次含有目的IP地址和端口号的套接字地址结构，未连接UDP套接字调用sendto时，每次都要复制

## 8.12 dg_cli函数（UDP已连接套接字修订版）

将前一个版本的dg_cli函数把它重写成调用connect

```c++
// 源码： udpcliserv/dgcliconnect.c

#include	"unp.h"

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int		n;
	char	sendline[MAXLINE], recvline[MAXLINE + 1];

	//调用connect，将未连接UDP套接字变为已连接UDP套接字
	Connect(sockfd, (SA *) pservaddr, servlen);

	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		//以write代替sendto
		Write(sockfd, sendline, strlen(sendline));

		//以read代替recvfrom
		n = Read(sockfd, recvline, MAXLINE);

		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
}
```

**服务器进程未运行情景**：

在主机macosx上运行该程序，并制定主机freebsd4的IP地址（它没有在端口号9877上运行相应的服务器程序）

```shell
macosx & udpcli04 172.24.37.94
hello,world #客户键入一行内容
read error : Connection refused #发送上一条数据报引来了服务器主机的ICMP错误
```

发送数据报引来了服务器主机的ICMP错误，该ICMP错误由内核映射成ECONNREFUSED错误，对应于由err_sys函数输出的消息串：“Connection refused”（连接被拒绝）

## 8.13 UDP缺乏流量控制（dg_echo套接字缓冲区修订版）

由于UDP缺乏流量控制，在服务器中，如果到达的数据报大于UDP套接字缓冲区，会被丢弃，而该“丢弃”行为不会给服务器进程或客户进程任何指示以说明数据报已丢失

**UDP套接字接收缓冲区**

由UDP给某个特定套接字排队的UDP数据报数目受限于该套接字接收缓冲区的大小

可以使用`SO_RCVBUF`套接字修改缓冲区大小。在FreeBSD下UDP套接字接收缓冲区的默认大小为42080字节（30个*1400字节数据报的空间）

增大套接字缓冲区的大小，服务器有望接收更多的数据报

下例中，把缓冲区大小设为240KB（220*1024）

```c++
// 源码： udpcliserv/dgecholoop2.c

#include	"unp.h"

static void	recvfrom_int(int);
static int	count;

void
dg_echo(int sockfd, SA *pcliaddr, socklen_t clilen)
{
	int			n;
	socklen_t	len;
	char		mesg[MAXLINE];

	//注册SIGINT信号的行为
	Signal(SIGINT, recvfrom_int);

	n = 220 * 1024;
	//设置缓冲区大小为240KB
	Setsockopt(sockfd, SOL_SOCKET, SO_RCVBUF, &n, sizeof(n));

	for ( ; ; ) {
		len = clilen;
		Recvfrom(sockfd, mesg, MAXLINE, 0, pcliaddr, &len);

		count++;
	}
}

//当服务器进程收到SIGINT信号时，输出count值
static void
recvfrom_int(int signo)
{
	printf("\nreceived %d datagrams\n", count);
	exit(0);
}
```

## 8.14 UDP中的外出接口的确定

已连接UDP套接字还可用来确定（获取）用于某个特定目的地的外出接口

**做法**：UDP被connect到一个指定IP地址后，调用getsockname得到本地IP地址和端口号

```c++
// 源码： udpcliserv/udpcli09.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	socklen_t			len;
	struct sockaddr_in	cliaddr, servaddr;

	if (argc != 2)
		err_quit("usage: udpcli <IPaddress>");

	sockfd = Socket(AF_INET, SOCK_DGRAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	servaddr.sin_port = htons(SERV_PORT);
	Inet_pton(AF_INET, argv[1], &servaddr.sin_addr);
	//connect到一个指定IP地址
	Connect(sockfd, (SA *) &servaddr, sizeof(servaddr));

	len = sizeof(cliaddr);
	//调用getsockname得到本地IP地址和端口号
	Getsockname(sockfd, (SA *) &cliaddr, &len);
	printf("local address %s\n", Sock_ntop((SA *) &cliaddr, len));

	exit(0);
}
```

**执行效果**：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/8-14-UDP%E4%B8%AD%E7%9A%84%E5%A4%96%E5%87%BA%E6%8E%A5%E5%8F%A3%E7%9A%84%E7%A1%AE%E5%AE%9A.png)

## 8.15 使用select函数的TCP和UDP回射服务器程序

使用select来复用TCP和UDP套接字的服务器程序

```c++
// 源码： udpcliserv/udpservselect01.c

/* include udpservselect01 */
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd, udpfd, nready, maxfdp1;
	char				mesg[MAXLINE];
	pid_t				childpid;
	fd_set				rset;
	ssize_t				n;
	socklen_t			len;
	const int			on = 1;
	struct sockaddr_in	cliaddr, servaddr;
	void				sig_chld(int);

		/* 4create listening TCP socket */
	listenfd = Socket(AF_INET, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	//设置SO_REUSEADDR套接字选项以防该端口上已有连接存在
	Setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &on, sizeof(on));
	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);

		/* 4create UDP socket */
	//创建一个UDP套接字
	udpfd = Socket(AF_INET, SOCK_DGRAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.sin_port        = htons(SERV_PORT);

	//绑定与TCP套接字相同的套接字，无需在调用Bind之前设置SO_REUSEADDR
	//因为TCP端口是独立于UDP端口的
	Bind(udpfd, (SA *) &servaddr, sizeof(servaddr));
/* end udpservselect01 */

/* include udpservselect02 */
	//给SIGCHLD建立信号处理程序，因为TCP连接将由某个子进程处理
	Signal(SIGCHLD, sig_chld);	/* must call waitpid() */

	//初始化描述符
	FD_ZERO(&rset);
	maxfdp1 = max(listenfd, udpfd) + 1;
	for ( ; ; ) {
		FD_SET(listenfd, &rset);
		FD_SET(udpfd, &rset);
		if ( (nready = select(maxfdp1, &rset, NULL, NULL, NULL)) < 0) {
			//子进程退出时，select返回-1，并且errno为EINTR,这里处理该错误
			if (errno == EINTR)
				continue;		/* back to for() */
			else
				err_sys("select error");
		}

		if (FD_ISSET(listenfd, &rset)) {
			len = sizeof(cliaddr);
			connfd = Accept(listenfd, (SA *) &cliaddr, &len);
	
			if ( (childpid = Fork()) == 0) {	/* child process */
				Close(listenfd);	/* close listening socket */
				str_echo(connfd);	/* process the request */
				exit(0);
			}
			Close(connfd);			/* parent closes connected socket */
		}

		if (FD_ISSET(udpfd, &rset)) {
			len = sizeof(cliaddr);
			n = Recvfrom(udpfd, mesg, MAXLINE, 0, (SA *) &cliaddr, &len);

			Sendto(udpfd, mesg, n, 0, (SA *) &cliaddr, len);
		}
	}
}
/* end udpservselect02 */
```

# 第11章 名字与地址转换

## 11.1 概述

**名字与数值间进行转换**： 

- **主机名字与IP地址之间进行转换**（**协议相关**，只支持**IPv4**）：
  - **主机名字转IP地址**：gethostbyname
  - **IP地址转主机名字**：gethostbyaddr
- **服务名字与端口号之间进行转换**： 
  - **服务名字转端口号**：getservbyname
  - **端口号转服务名字**：getservbyport
- **主机与服务名字转IP地址与端口号**： 
  - getaddrinfo（**协议无关**）
  - getaddrinfo的封装函数: 
    - host_serv
    - tcp_connect
    - tcp_listen
    - udp_client
    - udp_connect
    - udp_server
- **IP地址与端口号转主机与服务名字**：
  - getnameinfo

## 11.2 域名系统

域名系统（Domain Name System，DNS）主要用于主机名字与IP地址之间的映射

### 11.2.1 资源记录

DNS中的条目称为**资源记录**（Resource record，**RR**）

- **A**：A记录把一个**主机名映射成**一个32位的**IPv4地址**
  - 例子：unpbook.com域中关于主机freebsd的4条DNS记录，第一个是A记录

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_1_%E5%89%8D_DNS%E4%BE%8B%E5%AD%901.png)

- **AAAA**：称为“四A”记录的AAAA记录把一个**主机名映射为**一个128位的**IPv6地址**
- **PTR**：称为“指针记录”（Point record）的PTR记录把**IP地址映射成主机名**
  - 方法：32位地址的4个字节先反转顺序，每个字节转换成各自的十进制ASCII值（0~255）后，再添加in-addr.arpa，结果字符串用PTR查询
  - 例子：上例子主机freebsd的IPv4的PTR记录为：`254.32.106.12.in-addr.arpa`
- **MX**：MX记录把一个主机指定作为给定主机的“邮件交换器”
  - 例子：如A例子中的两个MX，其中5、10表示优先级，值越小优先级越高
- **CNAME**：CNAME代表"canonical name"（规范名字），它的常见用法是为常用的服务（ftp、www）指派CNAME记录
  - 例子：名为linux的主机有以下2个CNAME记录
    - `www.unpbook.com`的规范主机名是`linux.unpbook.com`
    - `ftp.unpbook.com`的规范主机名是`linux.unpbook.com`

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_1_%E5%89%8D_DNS%E4%BE%8B%E5%AD%902.png)

### 11.2.2 解析器和名字服务器

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_1_%E5%AE%A2%E6%88%B7_%E8%A7%A3%E6%9E%90%E5%99%A8%E5%92%8C%E5%90%8D%E5%AD%97%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9A%84%E5%85%B8%E5%9E%8B%E5%85%B3%E7%B3%BB.png)

- **应用程序**代码使用通常的函数调用来执行**解析器**中的代码（调用的典型函数是gethostbyname和gethostbyaddr）
- **解析器**代码通过读取其系统相关配置文件(通常是/etc/resolv.conf，包含本地名字服务器主机的IP地址)确定本组织机构的名字服务器**们**的所在位置
- 解析器使用UDP向**本地名字服务器**发出查询，如果本地名字服务器不知道答案，通常会使用UDP在整个因特网上查询其它名字服务器（如果答案太长，超出了UDP消息的承载能力，本地名字服务器和解析器会自动切换到TCP） 

### 11.2.3 DNS替代方法

不使用DNS也可以获取名字和地址信息。通常的替代方法有：

- 1.静态主机文件（通常是/etc/hosts文件）
- 2.网络信息系统（NIS）
- 3.轻权目录访问协议（LDAP）

系统管理员如何配置一个主机以使用不同类型的名字服务是实现相关的，但这些差异对开发人员来说，通常是透明的，只需调用诸如gethostbyname和gethostbyaddr这样的解析器函数

# 主机名字与IP地址之间的转换

## 11.3 gethostbyname函数

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_gethostbyname%E5%87%BD%E6%95%B0.png)

gethostbyname执行的是对A记录的查询，它只能返回IPv4地址

返回的指针指向hostent结构，该结构含有所查找主机的所有IPv4地址： 

```c
struct hostent{
    char *h_name;           //规范主机名
    char **h_aliases;       //主机别名
    int h_addrtype;         //主机地址类型：AF_INET
    int h_length;           //地址长度：4
    char **h_addr_list;     //IPv4地址
};
```

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_2_hostent%E7%BB%93%E6%9E%84%E5%92%8C%E5%AE%83%E6%89%80%E5%8C%85%E5%90%AB%E7%9A%84%E4%BF%A1%E6%81%AF.png)

当发生错误时，函数会设置全局变量h_errno为定义在<netdb.h>中的下列常值： 

- **HOST_NOT_FOUND**：
- **TRY_AGAIN**：
- **NO_RECOVERY**：
- **NO_DATA**(等同于**NO_ADDRESS**)：表示指定的名字有效，但没有A记录（只有MX记录的主机名就是这样的一个例子）

如今多数解析器提供hstrerror函数，该函数以某个h_errno值作为唯一参数，返回一个指向相应错误说明的const char *指针 

## 11.4 gethostbyaddr函数

该函数试图由一个**二进制**的IP地址找到相应的主机名，与gethostbyname的行为相反 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_gethostbyaddr%E5%87%BD%E6%95%B0.png)

- **addr**：实际上是一个指向存放IPv4地址的某个in_addr结构的指针 
- **len**：addr指向的in_addr结构的大小（对于IPv4地址为4） 
- **family**：AF_INET 
- **返回值**：返回一个指向hostent结构的指针，但是不同于gethostbyname，这里我们感兴趣的通常是存放规范主机名的h_name字段 

按照DNS的说法，gethostbyaddr在in_addr.arpa域中向一个名字服务器查询PTR记录

# 服务名字与端口号之间的转换

## 11.5 getservbyname和getservbyport函数

在程序代码中通常通过其名字而不是端口号来指代一个服务（如ftp、shell之类），而且从名字到端口号的映射关系保存在一个文件中（通常是/etc/services）。当端口号发生变动，我们只需要修改/etc/services文件中的某一行，而不必重新编译应用程序

/etc/services文件通常包含IANA维护的规范赋值列表的某个子集

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_services%E6%96%87%E4%BB%B6%E4%BE%8B%E5%AD%90.png)

> getservbyname函数

getservbyname函数根据给定名字查找相应服务

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_getservbyname%E5%87%BD%E6%95%B0.png)

- **servname**：服务名参数，必须指定
- **protoname**：协议，如果指定了，那么指定的服务必须有匹配的协议（如果protoname未指定而servname服务支持多个协议，那么返回哪个端口号取决于实现）

函数成功时返回指向servent结构的指针： 

```c
struct servent{
    char *s_name;       //规范服务名
    char **s_aliases;   //服务别名
    int s_port;         //服务对应的端口号（网络字节序）
    char *s_proto;      //使用的协议
};
```

> getservbyport函数

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_getservbyport%E5%87%BD%E6%95%B0.png)

- **port**：端口号，必须为网络字节序
- **protoname**：指定协议（有些端口号在TCP上用于一种服务，在UDP上却用于完全不同的另一种服务）

## 使用gethogetservbyport函数stbyname和getservbyname

```c
// 源码： names/daytimetcpcli1.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd, n;
	char				recvline[MAXLINE + 1];
	struct sockaddr_in	servaddr;
	struct in_addr		**pptr;
	struct in_addr		*inetaddrp[2];
	struct in_addr		inetaddr;
	struct hostent		*hp;
	struct servent		*sp;

	if (argc != 3)
		err_quit("usage: daytimetcpcli1 <hostname> <service>");

	//第一个命令行参数是主机名
	if ( (hp = gethostbyname(argv[1])) == NULL) {
		//如果gethostbyname查找失败
		//使用inet_aton判断argv[1]是否已经是ASCII格式的地址
		//inet_aton:将第一个参数所指的C字符串转换成32位的网络字节序二进制值，并通过第二个参数来存储
		if (inet_aton(argv[1], &inetaddr) == 0) {
			err_quit("hostname error for %s: %s", argv[1], hstrerror(h_errno));
		} else {
			//如果已经是ASCII格式的地址
			//构造一个由相应的地址构成的单元素列表
			inetaddrp[0] = &inetaddr;
			//表示达到尾元素，与h_addr_list做法一样
			inetaddrp[1] = NULL;
			pptr = inetaddrp;
		}
	} else {
		//gethostbyname查找成功
		pptr = (struct in_addr **) hp->h_addr_list;
	}

	//第二个参数是服务名
	if ( (sp = getservbyname(argv[2], "tcp")) == NULL)
		err_quit("getservbyname error for %s", argv[2]);

	//该循环为服务器主机的每个地址执行一次，直到connect成功或IP地址列表试完为止
	for ( ; *pptr != NULL; pptr++) {
		sockfd = Socket(AF_INET, SOCK_STREAM, 0);

		bzero(&servaddr, sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		servaddr.sin_port = sp->s_port;
		memcpy(&servaddr.sin_addr, *pptr, sizeof(struct in_addr));
		printf("trying %s\n",
			   Sock_ntop((SA *) &servaddr, sizeof(servaddr)));

		if (connect(sockfd, (SA *) &servaddr, sizeof(servaddr)) == 0)
			break;		/* success */
		err_ret("connect error");
		//如果connect失败，必须关闭套接字，并重新创建一个新的套接字使用
		close(sockfd);
	}
	if (*pptr == NULL)
		err_quit("unable to connect");

	while ( (n = Read(sockfd, recvline, MAXLINE)) > 0) {
		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
	exit(0);
}
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd, n;
	char				recvline[MAXLINE + 1];
	struct sockaddr_in	servaddr;
	struct in_addr		**pptr;
	struct in_addr		*inetaddrp[2];
	struct in_addr		inetaddr;
	struct hostent		*hp;
	struct servent		*sp;

	if (argc != 3)
		err_quit("usage: daytimetcpcli1 <hostname> <service>");

	//第一个命令行参数是主机名
	if ( (hp = gethostbyname(argv[1])) == NULL) {
		//如果gethostbyname查找失败
		//使用inet_aton判断argv[1]是否已经是ASCII格式的地址
		//inet_aton:将第一个参数所指的C字符串转换成32位的网络字节序二进制值，并通过第二个参数来存储
		if (inet_aton(argv[1], &inetaddr) == 0) {
			err_quit("hostname error for %s: %s", argv[1], hstrerror(h_errno));
		} else {
			//如果已经是ASCII格式的地址
			//构造一个由相应的地址构成的单元素列表
			inetaddrp[0] = &inetaddr;
			//表示达到尾元素，与h_addr_list做法一样
			inetaddrp[1] = NULL;
			pptr = inetaddrp;
		}
	} else {
		//gethostbyname查找成功
		pptr = (struct in_addr **) hp->h_addr_list;
	}

	//第二个参数是服务名
	if ( (sp = getservbyname(argv[2], "tcp")) == NULL)
		err_quit("getservbyname error for %s", argv[2]);

	//该循环为服务器主机的每个地址执行一次，直到connect成功或IP地址列表试完为止
	for ( ; *pptr != NULL; pptr++) {
		sockfd = Socket(AF_INET, SOCK_STREAM, 0);

		bzero(&servaddr, sizeof(servaddr));
		servaddr.sin_family = AF_INET;
		servaddr.sin_port = sp->s_port;
		memcpy(&servaddr.sin_addr, *pptr, sizeof(struct in_addr));
		printf("trying %s\n",
			   Sock_ntop((SA *) &servaddr, sizeof(servaddr)));

		if (connect(sockfd, (SA *) &servaddr, sizeof(servaddr)) == 0)
			break;		/* success */
		err_ret("connect error");
		//如果connect失败，必须关闭套接字，并重新创建一个新的套接字使用
		close(sockfd);
	}
	if (*pptr == NULL)
		err_quit("unable to connect");

	while ( (n = Read(sockfd, recvline, MAXLINE)) > 0) {
		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
	exit(0);
}
```

# 主机与服务名字转IP地址与端口号

## 11.6 getaddrinfo函数

gethostbyname和gethostbyaddr仅仅支持IPv4

getaddrinfo与协议无关，并且能处理**名字到地址**、**服务到端口**这两种转换。返回的不再是地址列表，返回的addrinfo结构中包含了一个指向sockaddr结构的指针，这些sockaddr结构随后可由套接字函数**直接使用**，因此将协议相关性完全隐藏在函数的内部 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_getaddrinfo%E5%87%BD%E6%95%B0.png)

- **hostname**：主机名或地址串
- **service**：服务名或十进制端口号数串
- **hints**：空指针或指向某个addrinfo结构的指针，调用者在这个结构中填入期望返回的信息类型的暗示
  - 如果hints参数是一个空指针，函数就假设ai_flags、ai_family、ai_socktype为0，ai_protocol为AF_UNSPEC
  - 调用者可以通过hints设置的字段有：ai_flags、ai_family、ai_socktype、ai_protocol
- **result**：指向addrinfo结构的指针，**返回值** 
  - 如果与hostname参数关联的地址有多个，那么适用于所请求地址族的每个地址都返回一个对应的结构 
  - 如果service参数指定的服务支持多个套接字类型，那么每个套接字类型都可能返回一个对应的结构 

```c
struct addrinfo{
    int ai_flags;               //0个或多个或在一起的AI_xxx值
    int ai_family;              //某个AF_xxx值
    int ai_socktype;            //某个SOCK_xxx值
    int ai_protocol;            //0或IPPROTO_xxx
    socklen_t ai_addrlen;       //ai_addr的长度
    char *ai_canonname;         //指向规范主机名的指针
    struct sockaddr *ai_addr;   //指向套接字地址结构的指针
    struct addrinfo *ai_next;   //指向链表中下一个addrinfo结构的指针
};
```

**ai_flags**成员可用的标志值及含义如下：

- **AI_PASSIVE**：套接字将用于被动打开
- **AI_CANONNAME**：告知getaddrinfo函数返回主机的规范名字（在链表中的第一个addrinfo结构的ai_canonname成员中）
- **AI_NUMERICHOST**：防止任何类型的名字到地址映射，hostname参数必须是一个地址串
- **AI_NUMERICSERV**：防止任何类型的名字到服务映射，service参数必须是一个十进制端口号数串
- **AI_V4MAPPED**：如果同时指定ai_family的值为AF_INET6，那么如果没有可用的AAAA记录，就返回与A记录对应的IPv4映射的IPv6地址
- **AI_ALL**：如果同时指定了AI_V4MAPPED，那么返回与AAAA记录对应的IPv6地址、与A记录对于的IPv4映射的IPv6地址
- **AI_ADDRCONFIG**：按照所在主机的配置选择返回地址类型

**例子**：假设主机freebsd4的规范名字是freebsd4.unpbook.com，并且它在DNS中有两个IPv4地址

调用getaddrinfo函数，如果函数成功，result指向的变量已被填入一个指针，指向的是由ai_next成员串起来的addrinfo结构链表（这些结构的先后顺序没有保证）： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_5_getaddrinfo%E8%BF%94%E5%9B%9E%E4%BF%A1%E6%81%AF%E7%9A%84%E5%AE%9E%E4%BE%8B.png)

getaddrinfo的**常见用法**：

- **TCP或UDP客户同时制定hostname和service** 
  - **TCP客户**：在一个循环中针对每个返回的IP地址，逐一调用socket和connect，直到有一个连接成功，或者所有地址尝试完毕 
  - **UDP客户**：由getaddrinfo填入的套接字地址结构用于调用sendto或connect。如果客户能够判断第一个地址看来不工作（或者在已连接的UDP套接字上收到出错消息，或者在未连接的套接字上经历消息接收超时），那么可以尝试其余地址 
- **典型的服务器只指定service而不指定hostname，同时在hints结构中指定AI_PASSIVE标志**。返回的套接字地址结构中应含有一个值为INADDR_ANY(对于IPv4)或IN6ADDR_ANY_INIT(对于IPv6)的IP地址 
  - 函数返回后，**TCP服务器**调用socket、bind和listen。如果服务器想要malloc另一个套接字地址结构以从accept获取客户的地址，那么返回的ai_addrlen值给出了这个套接字地址结构的大小 
  - **UDP服务器**将调用socket、bind和recvfrom。如果想要malloc另一个套接字地址结构以从recvfrom获取客户的地址，那么返回的ai_addrlen值给出了这个套接字地址结构的大小 
- **服务器程序使用select或poll函数让服务器进程处理多个套接字**：服务器将遍历由getaddrinfo返回的整个addrinfo结构链表，并为每个结构创建一个套接字，再使用select或poll 

如果**客户或服务器**清楚自己只处理一种类型的套接字，那么应该把hints结构的ai_socktype成员设置成SOCK_STREAM或SOCK_DGRAM 

getaddrinfo函数的**优点**：

- 协议无关
- 单个函数能够同时处理主机名和服务
- 所有返回信息都是**动态分配**的

getaddrinfo函数的**缺点**：必须先分配一个hints结构，把它清零后填写需要的字段，再调用getaddrinfo，然后遍历一个链表逐一尝试每个返回地址 

## 11.7 gai_strerror函数

如果getaddrinfo函数发生**错误**，该函数返回错误值，该值可以作为函数**gai_strerror()**的参数。调用gai_strerror函数可以得到一个描述错误信息的C字符串指针： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_gai_strerror%E5%87%BD%E6%95%B0.png)

**error的值**如下表：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_7_getaddrinfo%E8%BF%94%E5%9B%9E%E7%9A%84%E9%9D%9E0%E9%94%99%E8%AF%AF%E5%B8%B8%E5%80%BC.png)

## 11.8 freeaddrinfo函数

getaddrinfo函数返回的所有**存储空间都是动态获取**（譬如来自malloc调用）的，包括addrinfo结构、ai_addr结构和ai_canonname字符串，可以通过调用**freeaddrinfo**返**还给系统**： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_freeaddrinfo%E5%87%BD%E6%95%B0.png)

- **ai**：指向由getaddrinfo返回的第一个addrinfo结构（这个链表中所有的结构以及由它们指向的任何动态存储空间都被释放掉） 

## 11.11 host_serv函数

host_serv封装了函数getaddrinfo，不要求调用者分配并填写一个hints结构，该结构中的**地址族**和**套接字类型**字段作为参数： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_host_serv%E5%87%BD%E6%95%B0.png)

**host_serv函数**：

```c
// 源码： lib/host_serv.c

#include	"unp.h"

struct addrinfo *
host_serv(const char *host, const char *serv, int family, int socktype)
{
	int				n;
	struct addrinfo	hints, *res;

	//初始化一个hints结构
	bzero(&hints, sizeof(struct addrinfo));
	hints.ai_flags = AI_CANONNAME;	/* always return canonical name */
	hints.ai_family = family;		/* AF_UNSPEC, AF_INET, AF_INET6, etc. */
	hints.ai_socktype = socktype;	/* 0, SOCK_STREAM, SOCK_DGRAM, etc. */

	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		return(NULL);

	return(res);	/* return pointer to first on linked list */
}
```

## 11.12 tcp_connect函数

tcp_connect执行TCP客户的通常步骤：创建一个TCP套接字并连接到一个服务器 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_tcp_connect%E5%87%BD%E6%95%B0.png)

**tcp_connect函数**：

```c
// 源码： lib/tcp_connect.c

#include	"unp.h"

int
tcp_connect(const char *host, const char *serv)
{
	int				sockfd, n;
	struct addrinfo	hints, *res, *ressave;

	bzero(&hints, sizeof(struct addrinfo));
	//指定地址族为AF_UNSPEC
	hints.ai_family = AF_UNSPEC;
	//指定套接字类型为SOCK_STREAM
	hints.ai_socktype = SOCK_STREAM;

	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		err_quit("tcp_connect error for %s, %s: %s",
				 host, serv, gai_strerror(n));
	ressave = res;

	//尝试getinfo返回的每一个IP地址
	do {
		sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
		if (sockfd < 0)
			continue;	/* ignore this one */

		if (connect(sockfd, res->ai_addr, res->ai_addrlen) == 0)
			break;		/* success */

		Close(sockfd);	/* ignore this one */
	} while ( (res = res->ai_next) != NULL);

	if (res == NULL)	/* errno set from final connect() */
		err_sys("tcp_connect error for %s, %s", host, serv);

	//把所有动态分配的内存空间返回系统
	freeaddrinfo(ressave);

	return(sockfd);
}
```

**例子：时间获取TCP客户程序**

```c
// 源码： names/daytimetcpcli.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int				sockfd, n;
	char			recvline[MAXLINE + 1];
	socklen_t		len;
	struct sockaddr_storage	ss;

	if (argc != 3)
		err_quit("usage: daytimetcpcli <hostname/IPaddress> <service/port#>");

	//argv[1]：主机名或IP地址
	//argv[2]：服务名或端口号
	sockfd = Tcp_connect(argv[1], argv[2]);

	len = sizeof(ss);
	//取得服务器的协议地址并显示出来，为了在后面的例子中验证所用的协议
	Getpeername(sockfd, (SA *)&ss, &len);
	printf("connected to %s\n", Sock_ntop_host((SA *)&ss, len));

	while ( (n = Read(sockfd, recvline, MAXLINE)) > 0) {
		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
	exit(0);
}
```

## 11.13 tcp_listen函数

tcp_listen执行TCP服务器的通常步骤：创建一个TCP套接字，给它捆绑服务器的众所周知的端口，并允许接受外来的连接请求： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_tcp_listen%E5%87%BD%E6%95%B0.png)

**tcp_listen函数**：

```c
// 源码： lib/tcp_listen.c

#include	"unp.h"

int
tcp_listen(const char *host, const char *serv, socklen_t *addrlenp)
{
	int				listenfd, n;
	const int		on = 1;
	struct addrinfo	hints, *res, *ressave;

	bzero(&hints, sizeof(struct addrinfo));
	//套接字将用于被动打开，因为本函数供服务器使用
	hints.ai_flags = AI_PASSIVE;
	//指定地址族为AF_UNSPEC
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_STREAM;

	//如果不指定主机名host（对于想绑定通配地址的服务器通常如此）
	//AI_PASSIVE和AF_UNSPEC这两个暗示信息将会返回两个套接字地址结构
	//第一个是IPv6的，第二个是IPv4的（假定运行在一个双栈主机上）
	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		err_quit("tcp_listen error for %s, %s: %s",
				 host, serv, gai_strerror(n));
	ressave = res;

	//调用socket和bind函数，如果任何一个调用失败，那就忽略当前addrinfo结构而改用下一个
	do {
		listenfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
		if (listenfd < 0)
			continue;		/* error, try next one */

		//对TCP服务器总是设置SO_REUSEADDR套接字选项
		Setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &on, sizeof(on));
		if (bind(listenfd, res->ai_addr, res->ai_addrlen) == 0)
			break;			/* success */

		Close(listenfd);	/* bind error, close and try next one */
	} while ( (res = res->ai_next) != NULL);

	//检查是否失败（遍历完整个链表），如果失败则显示一个出错消息并终止
	if (res == NULL)	/* errno from final socket() or bind() */
		err_sys("tcp_listen error for %s, %s", host, serv);

	Listen(listenfd, LISTENQ);

	//通过addrlenp指针返回协议地址的大小
	if (addrlenp)
		*addrlenp = res->ai_addrlen;	/* return size of protocol address */

	freeaddrinfo(ressave);

	return(listenfd);
}
```

**例子：时间获取TCP服务器程序（不可指定协议）**

```c
// 源码： names/daytimesrv1.c

#include	"unp.h"
#include	<time.h>

int
main(int argc, char **argv)
{
	int				listenfd, connfd;
	socklen_t		len;
	char			buff[MAXLINE];
	time_t			ticks;
	struct sockaddr_storage	cliaddr;

	if (argc != 2)
		err_quit("usage: daytimetcpsrv1 <service or port#>");

    //第一个参数是空指针，由tcp_listen内部指定的协议族为AF_UNSPEC
    //在双栈主机上返回IPv6个IPv4套接字地址结构
	listenfd = Tcp_listen(NULL, argv[1], NULL);

	for ( ; ; ) {
		len = sizeof(cliaddr);
		connfd = Accept(listenfd, (SA *)&cliaddr, &len);
		printf("connection from %s\n", Sock_ntop((SA *)&cliaddr, len));

		ticks = time(NULL);
		snprintf(buff, sizeof(buff), "%.24s\r\n", ctime(&ticks));
		Write(connfd, buff, strlen(buff));

		Close(connfd);
	}
}

```

**例子：时间获取TCP服务器程序（可指定协议）**

该版本允许我们强制服务器使用某个给定的协议（IPv4或IPv6）：允许用户作为程序的命令行参数输入一个IP地址或主机名，根据输入值判断协议类型（getaddrinfo会自行判断）

```c
// 源码： names/daytimesrv2.c

#include	"unp.h"
#include	<time.h>

int
main(int argc, char **argv)
{
	int				listenfd, connfd;
	socklen_t		len, addrlen;
	char			buff[MAXLINE];
	time_t			ticks;
	struct sockaddr_storage	cliaddr;

    //主要修改在这里
	if (argc == 2)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 3)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: daytimetcpsrv2 [ <host> ] <service or port>");

	for ( ; ; ) {
		len = sizeof(cliaddr);
		connfd = Accept(listenfd, (SA *)&cliaddr, &len);
		printf("connection from %s\n", Sock_ntop((SA *)&cliaddr, len));

		ticks = time(NULL);
		snprintf(buff, sizeof(buff), "%.24s\r\n", ctime(&ticks));
		Write(connfd, buff, strlen(buff));

		Close(connfd);
	}
}
```

## 11.14 udp_client函数

创建未连接UDP套接字 ：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_udp_client%E5%87%BD%E6%95%B0.png)

- **saptr**：指向的套接字地址结构保存有服务器的IP地址和端口号，用于稍后调用sendto 
- **lenp**：saptr所指的套接字地址结构的大小。不能为空指针，因为任何sendto和recvfrom调用都需要知道套接字地址结构的长度 

**udp_client函数**：

```c
// 源码： lib/udp_client.c

#include	"unp.h"

int
udp_client(const char *host, const char *serv, SA **saptr, socklen_t *lenp)
{
	int				sockfd, n;
	struct addrinfo	hints, *res, *ressave;

	bzero(&hints, sizeof(struct addrinfo));
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_DGRAM;

	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		err_quit("udp_client error for %s, %s: %s",
				 host, serv, gai_strerror(n));
	ressave = res;

	do {
		sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
		if (sockfd >= 0)
			break;		/* success */
	} while ( (res = res->ai_next) != NULL);

	if (res == NULL)	/* errno set from final socket() */
		err_sys("udp_client error for %s, %s", host, serv);

	//用于分配一个套接字地址结构的内存空间
	*saptr = Malloc(res->ai_addrlen);
	memcpy(*saptr, res->ai_addr, res->ai_addrlen);
	*lenp = res->ai_addrlen;

	freeaddrinfo(ressave);

	return(sockfd);
}
```

**例子：时间获取UDP客户程序（可指定协议）**

```c
// 源码： names/daytimeudpcli1.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int				sockfd, n;
	char			recvline[MAXLINE + 1];
	socklen_t		salen;
	struct sockaddr	*sa;

	if (argc != 3)
		err_quit("usage: daytimeudpcli1 <hostname/IPaddress> <service/port#>");

	//获得一个UDP套接字，注意这里的salen
	sockfd = Udp_client(argv[1], argv[2], (void **) &sa, &salen);

	printf("sending to %s\n", Sock_ntop_host(sa, salen));

	Sendto(sockfd, "", 1, 0, sa, salen);	/* send 1-byte datagram */

	n = Recvfrom(sockfd, recvline, MAXLINE, 0, NULL, NULL);
	recvline[n] = '\0';	/* null terminate */
	Fputs(recvline, stdout);

	exit(0);
}
```

## 11.15 udp_connect函数

创建一个已连接UDP套接字： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_udp_connect%E5%87%BD%E6%95%B0.png)

因为已连接套接字改用write代替sendto，所以相比于udp_client，省略了套接字地址结构及长度参数 

**udp_connect函数**：

与tcp_connect的区别：

- UDP套接字上的connect调用不会发送任何东西到对端。如果存在错误（如对端不可达或指定端口上没有服务器），调用者就得等到向对端发送一个数据报后才能发现

```c
// 源码： lib/udp_server.c

#include	"unp.h"

int
udp_server(const char *host, const char *serv, socklen_t *addrlenp)
{
	int				sockfd, n;
	struct addrinfo	hints, *res, *ressave;

	bzero(&hints, sizeof(struct addrinfo));
	hints.ai_flags = AI_PASSIVE;
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_DGRAM;

	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		err_quit("udp_server error for %s, %s: %s",
				 host, serv, gai_strerror(n));
	ressave = res;

	do {
		sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
		if (sockfd < 0)
			continue;		/* error - try next one */

		if (bind(sockfd, res->ai_addr, res->ai_addrlen) == 0)
			break;			/* success */

		Close(sockfd);		/* bind error - close and try next one */
	} while ( (res = res->ai_next) != NULL);

	if (res == NULL)	/* errno from final socket() or bind() */
		err_sys("udp_server error for %s, %s", host, serv);

	if (addrlenp)
		*addrlenp = res->ai_addrlen;	/* return size of protocol address */

	freeaddrinfo(ressave);

	return(sockfd);
}
```

## 11.16 udp_server函数

创建一个用于UDP服务器的UDP套接字：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_udp_server%E5%87%BD%E6%95%B0.png)

**udp_server函数**：

与tcp_listen的区别：

- 没有调用listen
- 不设置SO_REUSEADDR，因为该套接字选项允许在支持多播的主机上把同一个UDP端口绑定到多个套接字上。既然UDP套接字没有TCP的TIME_WAIT状态的类似物，启动服务器时就没有设置这个套接字选项的必要

```c
// 源码： lib/udp_server.c

#include	"unp.h"

int
udp_server(const char *host, const char *serv, socklen_t *addrlenp)
{
	int				sockfd, n;
	struct addrinfo	hints, *res, *ressave;

	bzero(&hints, sizeof(struct addrinfo));
	hints.ai_flags = AI_PASSIVE;
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_DGRAM;

	if ( (n = getaddrinfo(host, serv, &hints, &res)) != 0)
		err_quit("udp_server error for %s, %s: %s",
				 host, serv, gai_strerror(n));
	ressave = res;

	do {
		sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
		if (sockfd < 0)
			continue;		/* error - try next one */

		if (bind(sockfd, res->ai_addr, res->ai_addrlen) == 0)
			break;			/* success */

		Close(sockfd);		/* bind error - close and try next one */
	} while ( (res = res->ai_next) != NULL);

	if (res == NULL)	/* errno from final socket() or bind() */
		err_sys("udp_server error for %s, %s", host, serv);

	if (addrlenp)
		*addrlenp = res->ai_addrlen;	/* return size of protocol address */

	freeaddrinfo(ressave);

	return(sockfd);
}
```

**例子：时间获取UDP服务器程序（可指定协议）**

```c
// 源码： names/daytimeudpsrv2.c

#include	"unp.h"
#include	<time.h>

int
main(int argc, char **argv)
{
	int				sockfd;
	ssize_t			n;
	char			buff[MAXLINE];
	time_t			ticks;
	socklen_t		len;
	struct sockaddr_storage	cliaddr;

	if (argc == 2)
		sockfd = Udp_server(NULL, argv[1], NULL);
	else if (argc == 3)
		sockfd = Udp_server(argv[1], argv[2], NULL);
	else
		err_quit("usage: daytimeudpsrv [ <host> ] <service or port>");

	for ( ; ; ) {
		len = sizeof(cliaddr);
		n = Recvfrom(sockfd, buff, MAXLINE, 0, (SA *)&cliaddr, &len);
		printf("datagram from %s\n", Sock_ntop((SA *)&cliaddr, len));

		ticks = time(NULL);
		snprintf(buff, sizeof(buff), "%.24s\r\n", ctime(&ticks));
		Sendto(sockfd, buff, strlen(buff), 0, (SA *)&cliaddr, len);
	}
}
```

# IP地址与端口号转主机与服务名字

## 11.17 getnameinfo

getaddrinfo的互补函数，以一个套接字地址为参数，返回描述其中的主机的一个字符串和描述其中的服务的另一个字符串

**sock_ntop和getnameinfo的差别**：前者不涉及DNS，只返回IP地址和端口号的一个可显示版本；后者通常尝试获取主机和服务的名字（PTR记录）

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_getnameinfo%E5%87%BD%E6%95%B0.png)

- **sockaddr**：指向套接字地址结构，包含了待转换为可读字符串的协议地址
- **addrlen**：sockaddr指向的套接字地址结构的大小
- **host**：指向存储转换得到的”主机名“信息的buf（调用者预先分配）
- **hostlen**：host指向的buf的大小（不想获得”主机名“信息则设为0）
- **serv**：指向存储转换得到的”服务名“信息的buf（调用者预先分配）
- **servlen**：serv指向的buf的大小（不想获得”服务名“信息则设为0）
- **flags**：标志，见下表

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_20_getnameinfo%E7%9A%84%E6%A0%87%E5%BF%97%E5%80%BC.png)

**对flag的解释**：

- **NI_DGRAM**：当知道处理的是数据报套接字时，调用者应设置NI_DGRAM标志，因为在套接字地址结构中给出的仅仅是IP地址和端口号，getnameinfo无法就此确定所用协议（TCP或UDP）
- **NI_NAMEREQD**：如果无法使用DNS反向解析出主机名，该标志将导致返回一个错误
- **NI_NOFQDN**：FQDN：全限定额主机名。例如：主机名是bigserver,域名是mycompany.com,那么FQDN就是bigserver.mycompany.com 。
  - 假设套接字地址结构中的IP地址为192.168.42.2。那么不设置本标志的返回的主机名为`aix.unpbook.com`，设置本标志的返回的主机名为`aix`
- **NI_NUMERICHOST**：告知getnameinfo不要调用DNS，而是以数值表达格式以字符串的形式返回IP地址（可能调用inet_ntop）实现
- **NI_NUMERICSERV**：指定以十进制数格式作为字符串返回端口号，以替代查找服务名
- **NI_NUMERICSSCOPE**：指定以数值格式作为字符串返回范围标识，以替代其名字
- 可以把其中各个标志逻辑或在一起

# 可重入函数

## 11.18 可重入函数

gethostbyname和gethostbyaddr是不可重入的函数，因为它们公用一个静态的hostent类型的名为host的变量（每个进程有一个副本）

可重入问题发生在**同一进程的线程之间**，信号处理函数可以看成是一个新的线程

在一个普通的UNIX进程中发生重入问题的**条件**是：从它的**主控制流**中和某个**信号处理函数**中同时调用gethostbyname或gethostbyaddr。当这个调用**信号处理函数**被调用时，该进程的**主控制流**被暂停以执行信号处理函数。如果**主控制流**被暂停时正处于执行gethostbyname期间（如该函数已经填写好host变量并即将返回），而且**信号处理函数**随后调用gethostbyname，那么该host变量将被重用，因为该进程中值存在该变量的单个副本。这么一来，原先由**主控制流**计算出的值被重写成了由**信号处理函数**调用计算出来的值

**注意一下几点**：

- 1.gethostbyname、gethostbyaddr、getservbyname、getservbyport这4个函数是**不可重入的**，因为它们都返回指向同一个静态结构的指针。**解决方法**：
  - 提供这4个函数的可重入版本，以_r结尾
  - 使用线程特定数据（26.5节）提供这些函数的可重入版本
- 2.inet_pton、inet_ntop是**可重入的**
- 3.inet_ntoa是**不可重入的**（支持线程的一些实现提供了使用线程特定数据的可重入版本）
- 4.getaddrinfo可重入的前提是由它调用的函数都可重入。即它应该调用可重入版本的gethostbynamee和getservbyname。本函数返回的结果全部存放在动态分配内存空间的原因之一就是允许它可重入
- 5.getnameinfo可重入的前提是由它调用的函数都可重入。即它应该调用可重入版本的gethostbyaddr和getservbyport。它的2个结果字符串（主机名、服务名）由调用者分配存储空间，从而允许它可重入
- 6.errno变量存在类似的问题（每个进程有一个副本）。例如：从close调用返回时把错误码存入errno到稍后由程序显示errno的值之间存在一个小的时间窗口，期间同一个进程内的另一个执行线程（如一个信号处理函数的某次调用）可能改变了errno的值。**解决方法**：
  - 在信号处理函数中不调用任何不可重入的函数
  - 可以把信号处理函数编写成预先保存并事后恢复errno的值加以避免修改
- 7.许多版本的I/O函数库是**不可重入的**，我们不应该在信号处理函数中调用标准I/O函数

## 11.19 gethostbyname_r和gethostbyaddr_r函数

有两种方法可以把**不可重入的函数改为可重入函数**：

- 1.把由不可重入函数填写并返回静态结构的做法改为**由调用者分配**再由可重入函数填写结构
  - 例子：把不可重入的gethostbyname改为可重入的gethostbyname_r
  - 缺点：比较复杂
- 2.**由可重入函数调用malloc**以动态分配内存空间
  - 例子：getaddrinfo使用该方法
  - 缺点：必须调用freeaddrinfo释放动态分配的内存空间

**gethostbyname_r和gethostbyaddr_r函数**

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_gethostbyname_r%E5%92%8Cgethostbyaddr_r.png)

- **result**：指向由调用者分配并由被调用函数填写的hostent结构。成功返回时本指针作为函数的返回值
- **buf**：指向由调用者分配并且大小为**buflen**的缓冲区。该缓冲区用于存放规范主机名、别名指针数组、各个名字字符串、地址指针数组、各个实际地址。由**result**指向的hostent结构中的所有指针都指向该缓冲区内部
- **h_errnop**：如果出错，错误码通过**h_errnop**指针而不是全局变量h_errno返回

## 11.21 其他网络相关信息

应用进程可能想要查找4类与网络相关的信息：**主机**、**网络**、**协议**、**服务**。大多数查找针对的是主机，一小部分查找针对的是服务，更小一部分查找针对的是网络和协议。所有4类信息都可以放在一个文件中，每类信息各定义有3个访问函数： 

- 1.函数getXXXent读出文件中的下一个表项，必要的话首先打开文件
- 2.函数setXXXent打开（如果尚未打开的话）并回绕文件
- 3.函数endXXXent关闭文件

每类信息都定义了各自的结构：hostent、netent、protoent、servent。这些结构定义在头文件<netdb.h>中 

除了顺序处理文件的get、set和end三个函数外，每类信息还提供一些**键值查找函数**（通过getXXXent函数实现），寻找与某个参数匹配的一个表项。这些键值查找函数具有形如getXXXbyYYY的名字

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_11_21_%E5%9B%9B%E7%B1%BB%E7%BD%91%E7%BB%9C%E7%9B%B8%E5%85%B3%E4%BF%A1%E6%81%AF.png)

**注意**：

- **只有主机和网络信息可通过DNS获取**，协议和服务信息总是从相应文件中读取 
- 如果使用DNS查找主机和网络信息，只有键值查找函数才有意义。如果调用gethostent，那么它仅仅读取/etc/hosts文件并避免访问DNS 

## 11.22 小结

gethostbyname和gethostbyaddr通常是**不可重入的函数**。这两个函数共享一个静态的结果结构，都返回指向该结构的一个指针

# 第14章 高级I/O函数

## 14.2 套接字超时

涉及套接字的I/O操作上设置超时的方法有以下3种

- 1.使用**alarm**，它在指定超时期满时产生SIGALRM信号（**适用于任何描述符**） 
  - 缺点：涉及信号处理，信号处理在不同实现上存在差异，而且可能干扰进程中现有的alarm信号
- 2.在**select**中阻塞等待I/O（select有内置的时间限制），以此替代直接阻塞在read或write调用上（**适用于任何描述符**） 
- 3.使用**SO_RCVTIMEO**和**SO_SNDTIMEO**套接字选项 （**仅使用于套接字描述符**） 
  - 缺点：并非所有实现都支持

上述3个技术都适用于输入和输出操作（read、write、recvfrom、sendto等）。但是对于connect（connect的**内置超时相当长，典型值为75s**）

- 1.使用SIGALRM设置（14.2.1详细介绍）
- 2.select用来在connect上设置超时的先决条件是相应套接字处于非阻塞模式 
- 3.`SO_RCVTIMEO`和`SO_SNDTIMEO`套接字选项对connect不适用 

### 14.2.1 使用SIGALRM为connect设置超时

```c
// 源码： lib/connect_timeo.c

/* include connect_timeo */
#include	"unp.h"

static void	connect_alarm(int);

int
connect_timeo(int sockfd, const SA *saptr, socklen_t salen, int nsec)
{
	Sigfunc	*sigfunc;
	int		n;

	//为SIGALRM建立一个新的信号处理函数
	//旧的信号处理函数保存在sigfunc中，以便在本函数结束时恢复它
	sigfunc = Signal(SIGALRM, connect_alarm);
	//报警时钟设置成由调用者指定的秒数
	//如果此前已经给本进程设置过报警时钟，那么alarm的返回值是这个报警时钟的当前
	//剩余秒数，否则alarm返回值为0
	//如果是已经设置过的情况，需要显示一个警告信息，因为我们推翻了先前设置的报警时钟
	if (alarm(nsec) != 0)
		err_msg("connect_timeo: alarm was already set");

	//如果本调用被中断（即返回EINTR错误），
	//就把errno值设为ETIMEOUT，同时关闭套接字，以防三路握手继续进行
	if ( (n = connect(sockfd, saptr, salen)) < 0) {
		close(sockfd);
		if (errno == EINTR)
			errno = ETIMEDOUT;
	}
	//通过以0位参数调用alarm关闭本进程的报警时钟
	alarm(0);					/* turn off the alarm */
	//恢复原来的信号处理函数
	Signal(SIGALRM, sigfunc);	/* restore previous signal handler */

	return(n);
}

static void
connect_alarm(int signo)
{
	//信号处理函数只是简单的返回，
	//本return语句将中断进程主控制流中那个未决的connect调用，
	//使得它返回一个EINTR错误，当捕获的信号为SIGALARM时，不设置SA_RESTART标志
	return;		/* just interrupt the connect() */
}
/* end connect_timeo */

void
Connect_timeo(int fd, const SA *sa, socklen_t salen, int sec)
{
	if (connect_timeo(fd, sa, salen, sec) < 0)
		err_sys("connect_timeo error");
}
```

**缺点**：

- 1.总能减少connect的超时期限，但是无法延长内核现有的超时期限（比如能比75小，但是不能更大） 
- 2.使用了系统调用的可中断能力，使得它们能够在内核超时发生之前返回，前提是：执行的是系统调用，并且能够直接处理由它们返回的EINTR错误 
- 3.在多线程中正确使用信号非常困难 

**建议**：只是在未线程化或简单线程化的程序使用本技术

### 14.2.2 使用SIGALRM位recvfrom设置超时

改写8.6节中的dg_cli函数，新的dg_cli函数通过调用alarm使得一旦在5秒内收不到任何应答就中断recvfrom

```c
// 源码： advio/dgclitimeo3.c

#include	"unp.h"

static void	sig_alrm(int);

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int	n;
	char	sendline[MAXLINE], recvline[MAXLINE + 1];

	//建立信号处理函数
	Signal(SIGALRM, sig_alrm);

	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		Sendto(sockfd, sendline, strlen(sendline), 0, pservaddr, servlen);

		//调用recvfrom前设置5秒的超时
		alarm(5);
		if ( (n = recvfrom(sockfd, recvline, MAXLINE, 0, NULL, NULL)) < 0) {
			//如果fecvfrom被信号处理函数中断，
			//输出一个信息并跳过本次循环执行下一次循环
			if (errno == EINTR)
				fprintf(stderr, "socket timeout\n");
			else
				err_sys("recvfrom error");
		} else {
			//如果读到一行来自服务器的文本，则关掉报警时钟并输出服务器应答
			alarm(0);
			recvline[n] = 0;	/* null terminate */
			Fputs(recvline, stdout);
		}
	}
}

static void
sig_alrm(int signo)
{
	//简单的返回，以发送让recvfrom返回小于0，从而中断被阻塞的recvfrom
	return;			/* just interrupt the recvfrom() */
}
```

### 14.2.3 使用select为recvfrom设置超时

这个名为readable_timeo的函数等待一个描述符最多在指定的描述内变为可读。本函数不执行度操作，它只是等待给定描述符变为可读，因此本函数适合于任何类型的套接字（TCP、UDP）

**readable_timeo函数**：

```c
// 源码： lib/readable_timeo.c

/* include readable_timeo */
#include	"unp.h"

int
readable_timeo(int fd, int sec)
{
	fd_set			rset;
	struct timeval	tv;

	FD_ZERO(&rset);
	//在读描述符集中打开与调用者给定描述符对应的位
	FD_SET(fd, &rset);

	//把调用者给定的等待秒数设置在一个timeval结构中
	tv.tv_sec = sec;
	tv.tv_usec = 0;

	//select等待该描述符变为可读，或发生超时
	//出错时为-1
	//超时发生时为0
	//正常则返回正值，为已就绪描述符的数目
	return(select(fd+1, &rset, NULL, NULL, &tv));
		/* 4> 0 if descriptor is readable */
}
/* end readable_timeo */

int
Readable_timeo(int fd, int sec)
{
	int		n;

	if ( (n = readable_timeo(fd, sec)) < 0)
		err_sys("readable_timeo error");
	return(n);
}
```

**readable_timeo使用例子**：

```c
// 源码： advio/dgclitimeo1.c

#include	"unp.h"

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int	n;
	char	sendline[MAXLINE], recvline[MAXLINE + 1];

	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		Sendto(sockfd, sendline, strlen(sendline), 0, pservaddr, servlen);
		
		if (Readable_timeo(sockfd, 5) == 0) {
			fprintf(stderr, "socket timeout\n");
		} else {
			n = Recvfrom(sockfd, recvline, MAXLINE, 0, NULL, NULL);
			recvline[n] = 0;	/* null terminate */
			Fputs(recvline, stdout);
		}
	}
}
```

### 14.2.4 使用SO_RCVTIMEO套接字选项为recvfrom设置超时

本例展示SO_RCVTIMEO套接字选项如何设置超时

本选项一旦设置到某个描述符（包括指定超时值），其超时设置将应用与该描述符上的所有读操作

**优势**：一次性设置选项。SIGALARM和select方法在每个操作发生之前必须做一些工作

SO_RCVTIMEO仅仅应用与读操作；SO_SNDTIMEO仅仅应用于写操作。都无法用于connect设置超时

```c
// 源码： advio/dgclitimeo2.c

#include	"unp.h"

void
dg_cli(FILE *fp, int sockfd, const SA *pservaddr, socklen_t servlen)
{
	int				n;
	char			sendline[MAXLINE], recvline[MAXLINE + 1];
	struct timeval	tv;

	tv.tv_sec = 5;
	tv.tv_usec = 0;
	//设置超时
	Setsockopt(sockfd, SOL_SOCKET, SO_RCVTIMEO, &tv, sizeof(tv));

	while (Fgets(sendline, MAXLINE, fp) != NULL) {

		Sendto(sockfd, sendline, strlen(sendline), 0, pservaddr, servlen);

		n = recvfrom(sockfd, recvline, MAXLINE, 0, NULL, NULL);
		if (n < 0) {
			//如果超时，recvfrom返回-1，且errno设为EWOULDBLOCK
			if (errno == EWOULDBLOCK) {
				fprintf(stderr, "socket timeout\n");
				continue;
			} else
				err_sys("recvfrom error");
		}

		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
}
```

## 14.7 排队的数据量

有时我们想要在不真正读取数据的前提下知道一个套接字上已有多少数据排队等着读取。**有3个技术可用于获悉已排队的数据量**： 

- 1.如果获悉已排队数据量的目的在于避免读操作阻塞在内核中（因为没有数据可读时我们还有其它事情可做），那么可以使用**非阻塞式I/O**（第16章）
- 2.如果既想查看数据，又想数据仍留在接收队列中以供本进程其它部分稍后读取，可以使用**MSG_PEEK**标志。如果想这样做，但是不能肯定是否真有数据可读，那么可以结合非阻塞套接字使用该标志，也可以组合使用MSG_DONTWAIT标志和MSG_PEEK标志 
  - 就一个字节流套接字而言，其接受队列中的数据量可能在两次相继的recv调用之间发生变化。因为在这两次调用之间TCP可能又收到一些数据
  - 就一个UDP套接字而言，即使另有数据报在两次调用之间加入接收队列，两个调用的返回值也完全相同（假设没有其他进程共享该套接字并从中读取数据） 
- 3.一些支持ioctl的FIONREAD命令。该命令的第3个ioctl参数是指向某个整数的一个指针，内核通过该整数返回的值就是套接字接收队列的当前字节数。该值是已排队字节的总和
  - 对于UDP套接字而言包括所有已排队的数据报（Berkely的实现还包括一个套接字地址结构的空间）

# Unix I/O函数

这部分介绍的函数称为Unix I/O。它们围绕描述符工作，**通常作为Unix内核中的系统调用实现** 

包括

- write和read
- sendto和recvfrom
- 14.3 recv和send函数
- 14.4 readv和writev函数
- 14.5 recvmsg和sendmsg函数

## 14.3 recv和send函数

这两个函数与标准的read和write的不同在于第4个参数： 

```c
#include <sys/socket.h>

ssize_t recv(int sockfd, void* buff, size_t nbytes, int flags);

ssize_t send(int sockfd, const void* buff, size_t nbytes, int flags);

//返回值：若成功则为读入或写出的字节数，若出错则为-1
```

- **flag**：参数的值或为0，或为下图中一个或兑个常值的**逻辑或**

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_6_IO%E5%87%BD%E6%95%B0%E7%9A%84flags%E5%8F%82%E6%95%B0.png)

**解释**：

- **MSG_DONTROUTE**： 告知内核目的主机在某个直接连接的本地网络上，因而无需执行路由表查找 
  - 既可以使用`MSG_DONTROUTE`标志针对单个输出操作开启
  - 也可以使用`SO_DONTROUTE`套接字选项针对某个给定套接字上的所有输出操作开启
- **MSG_DONTWAIT**： 在无需打开相应套接字的非阻塞标准下，把单个I/O操作临时指定为非阻塞，接着执行I/O操作，然后关闭非阻塞标志
- **MSG_OOB**：对于send，本标志指明即将发送带外数据；对于recv，指明即将读入的是带外数据而不是普通数据
- **MSG_PEEK**：本标志适用于**recv**和**recvfrom**。允许我们查看已经可读取的数据，而且系统不在recv或recvfrom返回后丢弃这些数据 
- **MSG_WAITALL**：告知内核不要在尚未读入请求数目的字节之前让一个读操作返回，但如果发生以下情况之一，相应的读函数仍然有可能返回比所请求字节数要少的数据
  - 捕获一个信号
  - 连接被终止
  - 套接字发生一个错误

**flags参数在设计上存在一个基本问题：它是值传递的，而不是一个值-结构参数。因此它只能用于从进程向内核传递标志。内核无法向进程传递标志**。对于TCP/IP协议这一点不成问题，因为TCP/IP几乎从不需要从内核向进程传回标志。然而随着OSI协议被加到4.3BSD Reno中，却提出了随输入操作向进程返送MSG_EOR标志的需求。4.3BSD  Reno做出的决定是保持常用输入函数(recv和recvfrom)的参数不变，而改变recvmsg和sendmsg的msghdr结构。这个决定同时意味着如果一个进程需要由内核更新标志，它就必须调用recvmsg，而不是recv或recvfrom 

## 14.4 readv和writev函数

与标准的read和write不同在于：readv和writev允许单个系统调用读入到或写出自一个或多个缓冲区，这些操作分别称为**分散读**和**集中写**，因为来自读操作的输入数据被分散到多个应用缓冲区中，而来自多个应用缓冲区的输出数据则被集中提供给单个写操作： 

```c
#include <sys/uio.h>

ssize_t readv(int filedes, const struct iovec *iov, int iovcnt);

ssize_t write(int filedes, const struct iovec *iov, int iovcnt);

//返回值：若成功则为读入或写出的字节数，若出错则为-1
```

- **iov**：指向某个iovec结构数组的一个指针 

```c
//定义在头文件<sys/uio.h>

struct iovec{
    void *iov_base; //buf的起始地址
    size_t iov_len; //buf的大小
};
```

- **iovcnt**：iovec结构数组中元素的个数存在某个限制，取决于具体实现。4.3BSD和Linux均最多允许1024个，而HP-UX最多允许2100个。POSIX要求在头文件<sys/uio.h>中定义IOV_MAX常值，其值至少为16

**一些特性**：

- 1.这两个函数可用于任何描述符，而不仅限于套接字
- 2.writev是一个**原子操作**，意味着对于一个基于记录的协议（如UDP）而言，一次writev调用只产生单个UDP数据报
  - **应用**：7.9节中提到的，一个4字节的writev后跟一个396字节的write可能触发Nagle算法，首选办法之一是针对这两个缓冲区调用writev

## 14.5 14.6 recvmsg和sendmsg、辅助数据

### recvmsg和sendmsg

这两个函数是最通用的I/O函数

可以把所有read、readv、recv、recvfrom调用替换成recvmsg调用

可以把所有write、writev、send、sendto调用替换成sendmsg调用

```c
# include<sys/socket.h>

ssize_t recvmsg(int sockfd, struct msghdr *msg, int flag);

ssize_t sendmsg(int sockfd, struct msghdr *msg, int flag);

//返回值：若成功则为读入或写出的字节数，若出错则为-1
```

- **msg**：指向一个msghdr结构，这个结构封装了大部分参数： 

```c
struct msghdr {
    /*
    ** msg_name和msg_namelen用于套接字未连接的场合（如UDP套接字）
    ** msg_name指向一个套接字地址结构，调用者在其中存放接收者或发送者的协议地址
    ** 如果无需指明协议地址（TCP或已连接UDP套接字）。msg_name应置为空指针
    ** namelen对于sendmsg是一个值参数，对于recvmsg却是一个值-结果参数
    */
	void *msg_name;
    socklen_t msg_namelen;
    /*
    ** msg_iov和msg_iovlen指定输入或输出缓冲区数组，
    ** 类似readv或writev的iov和iovcnt参数
    */
    struct iovec *msg_iov;
    int msg_iovlen; 
    /*
    ** msg_control和msg_controllen指定可选的辅助数据(控制信息)的位置和大小
    ** msg_controllen对于recvmsg是一个值-结果参数
    */
    void *msg_control;
    socklen_t msg_controllen;
    int msg_flags; 
};
```

对于recvmsg和sendmsg，必须区别**flags**参数（值参数）和msghdr结构（值-结果参数）的**msg_flags**成员

- 只有**recvmsg使用**msg_flags成员。recvmsg被调用时，flags参数被复制到msg_flags成员，并由内核使用其值驱动接收处理过程。内核还依据recvmsg的结果更新msg_flags成员的值
- **sendmsg则忽略**msg_flags成员，因为它直接使用flags参数驱动发送处理过程 

下图汇总了内核为输入和输出函数检查的flags参数值以及recvmsg可能返回的msg_flags： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_7_%E5%90%84%E7%A7%8DIO%E5%87%BD%E6%95%B0%E8%BE%93%E5%85%A5%E5%92%8C%E8%BE%93%E5%87%BA%E6%A0%87%E5%BF%97%E7%9A%84%E6%80%BB%E7%BB%93.png)

### 辅助数据

msghdr结构中的msg_control字段指向一个**辅助数据**，又称为**控制信息**

```c
// 定义在头文件<sys/socket.h>

struct cmsghdr{
    socklen_t cmsg_len; //该辅助对象的长度（结构体+数据）
    int cmsg_level;     //协议
    int cmsg_type;      //协议相关的辅助数据类型
};
```

cmsg_level和cmsg_type的取值和说明如下表： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_11_%E8%BE%85%E5%8A%A9%E6%95%B0%E6%8D%AE%E7%94%A8%E9%80%94%E7%9A%84%E6%80%BB%E7%BB%93.png)

**辅助数据由一个或多个辅助数据对象构成。每个辅助数据对象用一个cmsghdr结构表示**

- **msghdr结构**的msg_control指向第一个辅助数据对象
- **msghdr结构**的msg_controllen指定了辅助数据(即所有辅助数据对象)的总长度
- **cmsghdr结构**的msg_len是包括这个结构在内的字节数（结构体+数据）
- 填充字节
  - 在**cmsghdr结构**的cmsg_type成员和实际数据之间可以有填充字节
  - 从数据结尾处到下一个辅助数据对象之前也可以有填充字节

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_12_%E5%8C%85%E5%90%AB%E4%B8%A4%E4%B8%AA%E8%BE%85%E5%8A%A9%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%BE%85%E5%8A%A9%E6%95%B0%E6%8D%AE.png)

下面**5个宏**可以对应用程序屏蔽可能出现的填充字节，以简化对辅助数据的处理： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_13_%E5%90%8E_5%E4%B8%AA%E8%BE%85%E5%8A%A9%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E7%9A%84%E5%AE%8F.png)

**GMSG_LEN与GMSG_SPACE的区别**：

- GMSG_LEN：不计辅助数据对象中数据部分之后可能的填充字节，因而返回的是用于存放在cmsg_len成员中的值
- GMSG_SPACE：计上结尾处可能的填充字节，因而返回的是为辅助数据对象动态分配空间的大小

**5个宏的使用例子**

```c
//伪代码

struct msghdr msg;
struct cmsghdr* cmsgptr;

for( cmsghdr = CMSG_FIRSTHDR(&msg); cmsghdr != NULL; cmsghdr = CMSG_NXTHDR(&msg,cmsgptr)){
	u_char *ptr;
    ptr = CMSG_DATA(cmsghdr);
}
```

**下面以一个例子说明msghdr结构**：

- 假设进程即将对一个UDP套接字调用recvmsg，在调用前为这个套接字设置了IP_RECVDSTADDR套接字选项，以接收所读取UDP数据报的目的IP地址。调用时，msghdr结构如下：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_8_%E5%AF%B9%E4%B8%80%E4%B8%AAUDP%E5%A5%97%E6%8E%A5%E5%AD%97%E8%B0%83%E7%94%A8recvmsg%E6%97%B6%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.png)

- 接着假设从198.6.38.100端口2000到达一个170字节的UDP数据报，它的目的地址是我们的UDP套接字，目的IP地址为206.168.112.96。recvfrom返回时，msghdr结构如下： 

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_14_9_recvmsg%E8%BF%94%E5%9B%9E%E6%97%B6%E5%AF%B9%E5%9B%BE14-8%E7%9A%84%E6%9B%B4%E6%96%B0.png)

# 标准I/O函数

## 14.8 套接字和标准I/O

区别于**Unix I/O**,执行I/O的另一个方法是使用**标准I/O函数库**，这个函数库由**ANSI C**标准规范，意在便于移植到支持ANSI C的非Unix系统上 

**注意事项**：标准I/O函数库处理直接使用Unix I/O函数时必须考虑的一些细节，譬如自动缓冲输入流和输出流。不幸的是，对于流的缓冲处理可能导致同样必须考虑的一组新的问题 

标准I/O函数库可用于套接字，不过需要考虑以下几点： 

- 1.通过调用**fdopen**，可以从任何一个描述符创建出一个标准I/O流。类似地，通过调用**fileno**，可以获取一个给定标准I/O流对应的描述符 
- 2.TCP和UDP套接字是全双工的。标准I/O流也可以是全双工的：只要以r+类型(读写)打开流即可，但是在这样的流上 
  - 必须在调用一个**输出函数**之后，插入一个fflush、fseek、fsetpos或rewind调用才能接着调用一个**输入函数**
  -  必须在调用一个**输入函数**之后，插入一个fflush、fseek、fsetpos或rewind调用才能接着调用一个**输出函数**，除非输入函数遇到一个EOF 
  - 但是，fflush、fseek、fsetpos函数都调用lseek，而lseek用在套接字上只会失败 

**解决方法**：解决上述全双工问题的最简单方法是为一个给定套接字打开两个标准I/O流：一个用于读，一个用于写。但是**存在缓冲区问题** 

**例子：使用标准I/O的str_echo函数**

```c
// 源码： advio/str_echo_stdio02.c

#include	"unp.h"

void
str_echo(int sockfd)
{
	char		line[MAXLINE];
	FILE		*fpin, *fpout;

	//创建输入流
	fpin = Fdopen(sockfd, "r");
	//创建输出流
	fpout = Fdopen(sockfd, "w");
	
    //读到FIN，则返回空指针
	while (Fgets(line, MAXLINE, fpin) != NULL)
		Fputs(line, fpout);
}
```

以该版本的str_echo运行我们的**服务器**，然后运气其客户，得到以下结果：

```shell
#客户端
hpux % tcpcli02 206.168.112.96
hello,world #键入本行，但无回射输出
and hi      #再键入本行，仍无回射输出
hello??     #再键入本行，仍无回射输出
ctrl+D      #键入EOF
hello,world #至此才输出那三个回射行
and hi
hello??
```

服务器直到我们键入EOF字符才回射所有文本行的原因在于这里存在一个缓冲区问题，以下是实际发生的步骤：

- 1.客户键入第一行输入文本，它被发送到服务器
- 2.服务器用fgets读入文本，再用fputs回射本行
- 3.服务器的标准I/O流被标准I/O函数库**完全缓冲**。意味着该函数库把回射行复制到输出流的标准I/O缓冲区，但是不把该缓冲区中的内容写到描述符，因为**缓冲区未满**
- 4.客户键入第二、第三行，行为类似
- 5.客户键入EOF，致使客户str_cli函数调用shutdown（SHUT_WR），从而发送一个FIN到服务器
- 6.服务器TCP收到这个FIN，它被fgets读入，致使fgets返回一个空指针
- 7.str_echo函数返回到服务器main函数，子进程通过调用exit终止
- 8.C库函数exit调用标准I/O清理函数。之前由我们的fputs调用填充入输出缓冲区中的未满内容现在被输出
- 9.服务器子进程终止，它关闭已连接套接字，从而发送FIN到客户，完成TCP四分组终止序列
- 10.客户的str_cli函数收取并输出服务器回射的三行文本
- 11.客户的str_cli接着在其套接字上收到一个EOF，客户于是终止

这里的问题出在服务器中由标准I/O函数库自动执行的缓冲之上

标准I/O函数库执行以下三类缓冲

- 1.**完全缓冲**：意味着只在出现下列情况时才发生I/O：**缓冲区满**、**进程显示调用fflush**、**进程调用exit终止自身** 
  - 标准I/O缓冲区的通常大小为8192字节
- 2.**行缓冲**：意味着只在出现下列情况时才发生I/O：**碰到一个换行符**、**进程调用fflush**、**进程调用exit终止自身** 
- 3.**不缓冲**：意味着**每次调用标准I/O输出函数都发生I/O** 

标准I/O函数库的大多数Unix实现使用如下规则 

- 1.**标准错误**输出总是**不缓冲** 
- 2.**标准输入**和**标准输出**则**完全缓冲**，除非他们**指代终端设备**（这种情况下**行缓冲**） 
- 3.所有**其他I/O流**都是**完全缓冲**，除非他们**指代终端设备**（这种情况下**行缓冲**） 

由于套接字不是终端设备，因此服务器的str_echo的上述问题就在于**输出流是完全缓冲的**

**解决方法**：

- 1.通过**调用setbuf**迫使这个输出流变为**行缓冲**（易于犯错，与Nagle算法的交互成问题）
- 2.每次调用fputs之后通过**调用fflush**强制输出每个回射行（易于犯错，与Nagle算法的交互成问题）
- 3.彻底避免在套接字上使用标准I/O函数库

# 第15章 Unix域套接字

Unix域协议并不是一个实际的协议族，而是在**单个主机上**执行客户/服务器通信的一种方法

所用API就是在不同主机上执行客户/服务器通信所有的API（套接字API）

Unix域协议是进程间通信（IPC）的方法之一

Unix域提供**两类套接字**：字节流套接字(类似TCP)、数据报套接字(类似UDP) 

使用Unix域套接字有以下**3个理由**： 

- 1.Unix域套接字往往比通信两端位于同一主机的TCP套接字快出一倍 
- 2.Unix域套接字可用于在同一主机上的不同进程之间传递描述符
- 3.Unix域套接字较新的实现把客户的凭证(用户IP和组ID)提供给服务器，从而能够提供额外的安全检查措施 

## 15.2 Unix域套接字地址结构

```c
// 定义在头文件<sys/un.h>

struct sockaddr_un{
    sa_family_t sun_family;     /*AF_LOCAL*/
    char sun_path[104];         /*null-terminated pathname*/
};
```

**Unix域协议地址**：用于标识客户和服务器的协议地址是普通文件系统中的路径名。这些路径名不是普通Unix文件：除非把它们和Unix域套接字关联起来，否则无法读写这些文件

- **sun_path**：Unix域协议地址，存放在其中的路径名必须以空字符结尾。未指定地址以空字符串作为路径名指示（即sun_path[0]值为0的地址结构），它等价于IPv4的INADDR_ANY常值和IPv6的IN6ADDR_ANY_INIT常值 

**SUN_LEN宏**：以一个指向sockaddr_un结构的指针为参数并返回该结构的长度，其中包括路径名中非空字节数 

**例子：Unix域套接字的bind调用**

```c
// 源码： unixdomain/unixbind.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	socklen_t			len;
	struct sockaddr_un	addr1, addr2;

	if (argc != 2)
		err_quit("usage: unixbind <pathname>");

	sockfd = Socket(AF_LOCAL, SOCK_STREAM, 0);

	//如果文件系统中已存在该路径名，bind将会失败
	//调用unlink删除这个路径名，以防止它已经存在，
	//如果它不存在，unlink将返回一个我们要将其忽略的错误
	unlink(argv[1]);		/* OK if this fails */

	bzero(&addr1, sizeof(addr1));
	addr1.sun_family = AF_LOCAL;
	strncpy(addr1.sun_path, argv[1], sizeof(addr1.sun_path)-1);
	Bind(sockfd, (SA *) &addr1, SUN_LEN(&addr1));

	len = sizeof(addr2);
	Getsockname(sockfd, (SA *) &addr2, &len);
	printf("bound name = %s, returned len = %d\n", addr2.sun_path, len);
	
	exit(0);
}
```

**运行结果**

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_15_2_%E5%90%8E_Unix%E5%9F%9F%E5%A5%97%E6%8E%A5%E5%AD%97%E7%9A%84bind%E8%B0%83%E7%94%A8%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C.png)

- POSIX规定路径名的文件访问权限应该根据umask修正
- `ls -l`中可以看到，该路径名的文件类型显示为s（srwxr-xr-x中的第一个字母）
- `ls -lF`中可以看到，路径名之后添加了一个等号`=`
  - `-F`的作用：在每一个文件的末尾加上一个字符说明该文件的类型。`@`表示符号链接、`|`表示FIFOS、`/`表示目录、`=`表示套接字 

# Unix域相关函数

## 15.3 socketpair函数

socketpair函数创建2个随后连接起来的套接字，本函数仅适用于Unix域套接字

```c
#include <sys/socket.h>

int socketpair(int family, int type, int protocol, int sockfd[2]);

//返回值：若成功则为非0，若出错则为-1
```

- **family**：必须为AF_LOCAL 
- **protocol**：必须为0 
- **type**：既可以是SOCK_STREAM，也可以是SOCK_DGRAM
  - 使用SOCK_STREAM得到的结果称为流管道，它是全双工的
- **sockfd**：新创建的两个套接字描述符作为sockfd[0]和sockfd[1]返回
  - 这样创建的2个套接字不曾命名，也就是说其中没有涉及隐式的bind调用

## 15.4 套接字函数

用于Unix域套接字时，套接字函数中存在一些差异和限制。尽量列出POSIX的要求，并指出并非所有实现目前都已达到这个级别： 

- 1.由bind创建的路径名默认访问权限应为0777（属主用户、组用户和其他用户都可读、可写、可执行），并按当前umask值进行修正
- 2.与Unix域套接字关联的路径名应该是一个绝对路径，而不是一个相对路径
  - 不用相对路径的原因：解析依赖于调用者的当前工作目录。即要是服务器捆绑一个相对路径，客户就得在与服务器相同的目录中（或必须知道这个目录）才能成功调用connect或sendto
- 3.在connect调用中指定的路径名必须是一个当前绑定在某个打开的Unix域套接字上的路径名，而且它们的套接字类型也必须一致。出错条件包括：
  - 该路径名已存在却不是一个套接字
  - 该路径名已存在且是一个套接字，不过没有与之关联的打开的描述符
  - 该路径名已存在且是一个打开的套接字，不过类型不符（Unix域字节流套接字、Unix域数据报套接字）
- 4.调用connect连接一个Unix域套接字涉及的权限测试等同于调用open以只写方式访问相应的路径名
- 5.Unix域**字节流套接字**类似TCP套接字：它们都为进程提供一个无记录边界的字节流接口
- 6.Unix域**数据报套接字**类似于UDP套接字：都提供一个保留记录边界的不可靠的数据报服务
- 7.如果对于某个Unix域字节流套接字的connect调用发现这个监听套接字的队列已满，调用就立即返回一个ECONNREFUSED错误（对于TCP，监听套接字会忽略新到达的SYN，而TCP连接发起端将数次发送SYN进行重试）
- 8.在一个**未绑定**的Unix域套接字上**发送数据报**不会自动给这个套接字捆绑一个路径名（在一个未绑定的UDP套接字上发送UDP数据报导致给这个套接字捆绑一个临时端口）
- 9.类似8，对于某个Unix域数据报套接字的connect调用不会给本套接字捆绑一个路径名

## 15.5 Unix域字节流客户/服务器程序

**服务器**：

```c
// 源码： unixdomain/unixstrserv01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	socklen_t			clilen;
	struct sockaddr_un	cliaddr, servaddr;
	void				sig_chld(int);

	listenfd = Socket(AF_LOCAL, SOCK_STREAM, 0);

	//#define UNIXSTR_PATH "/tmp/unix.str"
	unlink(UNIXSTR_PATH);
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sun_family = AF_LOCAL;
	strcpy(servaddr.sun_path, UNIXSTR_PATH);
	
	Bind(listenfd, (SA *) &servaddr, sizeof(servaddr));

	Listen(listenfd, LISTENQ);

	Signal(SIGCHLD, sig_chld);

	for ( ; ; ) {
		clilen = sizeof(cliaddr);
		if ( (connfd = accept(listenfd, (SA *) &cliaddr, &clilen)) < 0) {
			if (errno == EINTR)
				continue;		/* back to for() */
			else
				err_sys("accept error");
		}

		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	/* close listening socket */
			str_echo(connfd);	/* process request */
			exit(0);
		}
		Close(connfd);			/* parent closes connected socket */
	}
}
```

**客户**：

```c
// 源码： unixdomain/unixstrcli01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_un	servaddr;

	sockfd = Socket(AF_LOCAL, SOCK_STREAM, 0);

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sun_family = AF_LOCAL;
	//#define UNIXSTR_PATH "/tmp/unix.str"
	strcpy(servaddr.sun_path, UNIXSTR_PATH);
	//注意，Unix域字节流套接字connect之前可以不绑定
	Connect(sockfd, (SA *) &servaddr, sizeof(servaddr));

	str_cli(stdin, sockfd);		/* do it all */

	exit(0);
}
```

## 15.6 Unix域数据报客户/服务器程序

**服务器**：

```c
// 源码： unixdomain/unixdgserv01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_un	servaddr, cliaddr;

	sockfd = Socket(AF_LOCAL, SOCK_DGRAM, 0);

	unlink(UNIXDG_PATH);
	bzero(&servaddr, sizeof(servaddr));
	servaddr.sun_family = AF_LOCAL;
	strcpy(servaddr.sun_path, UNIXDG_PATH);

	Bind(sockfd, (SA *) &servaddr, sizeof(servaddr));

	dg_echo(sockfd, (SA *) &cliaddr, sizeof(cliaddr));
}
```

**客户**:

```c
// 源码： unixdomain/unixdgcli01.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd;
	struct sockaddr_un	cliaddr, servaddr;

	sockfd = Socket(AF_LOCAL, SOCK_DGRAM, 0);

	bzero(&cliaddr, sizeof(cliaddr));		/* bind an address for us */
	cliaddr.sun_family = AF_LOCAL;
	//函数原型：char * tmpnam(char * ptr)
	//作用：系统自己创建一个文件，文件名系统自己给定。并且返回这个文件的路径名指针
	//参数ptr要求是一个指向一个长度至少是L_tmpnam个字符的数组，或者ptr为NULL
	//为NULL时，系统在一个静态区中存放新建文件的路径名。该静态区是公用的，
	//下一次ptr为NULL，调用该函数时，新的文件路径名仍存在这个静态区中
	strcpy(cliaddr.sun_path, tmpnam(NULL));

	//与UDP客户不同的是，Unix域数据报必须显示bind一个路径到套接字，
	//这样服务器才会有能回射应答的路径名
	Bind(sockfd, (SA *) &cliaddr, sizeof(cliaddr));

	bzero(&servaddr, sizeof(servaddr));	/* fill in server's address */
	servaddr.sun_family = AF_LOCAL;
	strcpy(servaddr.sun_path, UNIXDG_PATH);

	dg_cli(stdin, sockfd, (SA *) &servaddr, sizeof(servaddr));

	exit(0);
}
```

## 15.7 描述符传递

从一个进程到另一个进程传递打开的描述符时，通常有以下情形：

- 1.fork调用返回后，子进程分享父进程的所有打开的描述符
  - 过程：父进程先打开一个描述符，再调用fork，然后父进程关闭这个描述符，子进程则处理这个描述符
  - 无法做到：子进程打开一个描述符并把它传递给父进程
- 2.exec调用执行后，所有描述符通常保持打开状态不变

**前的Unix系统提供了用于从一个进程向任一其他进程传递任一打开的描述符的方法**：两个进程之间无需存在亲缘关系（如父子进程关系）。 这种技术要求：

- 1.首先在这两个进程之间创建一个Unix域套接字 
- 2.然后使用sendmsg跨这个套接字发送一个特殊消息 

这个消息由内核来专门处理，会把打开的描述符从发送进程传递到接收进程 



在两个进程之间传递描述符涉及的步骤如下：

- 1.创建一个字节流的或数据报的Unix域套接字 
  - **父子关系**：如果目标是fork一个子进程，让子进程打开待传递的描述符，再把它传递会父进程，那么父进程可以预先调用socketpair创建一个用于在父子进程之间交换描述符的流管道 
  - **无亲缘关系**：
    - 使用Unix域流字节套接字：服务器进程必须创建一个Unix域字节流套接字，bind一个路径名到该套接字，以允许客户进程connect到该套接字。然后客户可以向服务器发送一个打开某个描述符的请求，服务器再把该描述符通过Unix域套接字传递回客户 
    - 使用Unix域数据报套接字：没什么好处，而且数据报还存在被丢弃的可能，不推荐
- 2.发送进程调用返回描述符的任一Unix函数打开一个描述符
  - 函数可以是：open、pipe、mkfifo、socket、accept
  - 在进程之间传递的描述符**不限类型**
- 3.发送进程创建一个msghdr结构，其中含有待传递的描述符。POSIX规定描述符作为辅助数据（msghdr结构的msg_control成员）发送。发送进程**调用sendmsg**跨来自步骤1的Unix域套接字发送该描述符，然后改描述符进入“在飞行中”状态。即使发送进程在调用sendmsg之后但在接收进程调用recvmsg之前关闭了该描述符，对于接收进程它仍保持打开状态。发送一个描述符会使该描述符的引用计数加1
- 4.接收进程进程调用recvmsg接收来自步骤1的Unix域套接字上接收这个描述符。这个描述符在接收进程中的描述符号不同于它再发送进程中的描述符号是正常的。传递一个描述符不是传递一个描述符号，而是涉及在接收进程中创建一个新的描述符，这个新描述符和发送进程中的描述符指向内核中相同的文件表项

**注意**：

- 1.客户和服务器之间必须存在某种应用协议，以便描述符的接收进程预先知道何时期待接收。如果接收进程调用recvfrom时没有分配用于接收描述符的空间，而且之前已有一个描述符被传递并正等着被读取，这个早先传递的描述符就会被关闭
- 2.在期待接收描述符的recvmsg调用中应该避免使用MSG_PEEK标志

> 描述符传递的例子

给出一个描述符传递的例子：名为mycat的程序，通过命令行参数得到一个路径名，打开这个文件，再把文件的内容复制到标准输出。mycat程序调用my_open函数，my_open创建一个流管道，并调用fork和exec启动执行另一个openfile程序，期待输出的文件由openfile程序打开。openfile程序随后必须把打开的描述符通过流管道传递回给父进程

**步骤**：

- 1.通过调用socketpair创建一个流管道后的mycat进程

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_15_7_%E4%BD%BF%E7%94%A8socketpair%E5%88%9B%E5%BB%BA%E6%B5%81%E7%AE%A1%E9%81%93%E5%90%8E%E7%9A%84mycat%E8%BF%9B%E7%A8%8B.png)

- 2.mycat进程调用fork，子进程再调用exec执行openfile程序，父进程关闭[1]描述符，子进程关闭[0]描述符
  - 子进程调用exec时，向openfile传递a.待开打文件的路径名；b.打开方式；c.流管道某一端的描述符号

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_15_8_%E5%90%AF%E5%8A%A8%E6%89%A7%E8%A1%8Copenfile%E7%A8%8B%E5%BA%8F%E5%90%8E%E7%9A%84mycat%E8%BF%9B%E7%A8%8B.png)

- 3.openfile程序在通过流管道发送回打开的描述符后便终止，openfile的退出状态告知父进程文件能否打开，若不能则同时告知发生了什么类型的错误

**通过执行另一个程序来打开文件的优势**：另一个程序可以是一个setuid到root的程序，能够打开我们通常没有打开权限的文件。该程序能够把通常的Unix权限概念（用户、用户组和其他用户）扩展到它想要的任何形式的访问检查

**mycat程序**：

```c
//源码： unixdomain/mycat.c

#include	"unp.h"

int		my_open(const char *, int);

int
main(int argc, char **argv)
{
	int		fd, n;
	char	buff[BUFFSIZE];

	if (argc != 2)
		err_quit("usage: mycat <pathname>");

	if ( (fd = my_open(argv[1], O_RDONLY)) < 0)
		err_sys("cannot open %s", argv[1]);

	while ( (n = Read(fd, buff, BUFFSIZE)) > 0)
		Write(STDOUT_FILENO, buff, n);

	exit(0);
}
```

**my_open函数**：

```c
// 源码： unixdomain/myopen.c

#include	"unp.h"

int
my_open(const char *pathname, int mode)
{
	int			fd, sockfd[2], status;
	pid_t		childpid;
	char		c, argsockfd[10], argmode[10];

	//创建一个流管道sockfd[0]和sockfd[1]
	Socketpair(AF_LOCAL, SOCK_STREAM, 0, sockfd);

	if ( (childpid = Fork()) == 0) {		/* child process */
		Close(sockfd[0]);
		//sockfd[1]的描述符号格式化输出到argsockfd
		//打开方式mode格式化输出到argmode
		//调用snprintf进行格式化输出时因为exec的参数必须是字符串
		snprintf(argsockfd, sizeof(argsockfd), "%d", sockfd[1]);
		snprintf(argmode, sizeof(argmode), "%d", mode);
		//调用execl执行openfile程序
		//该函数不会返回，除非它发生错误
		//一旦成功，openfile程序的main函数就开始执行
		execl("./openfile", "openfile", argsockfd, pathname, argmode,
			  (char *) NULL);
		err_sys("execl error");
	}

	/* parent process - wait for the child to terminate */
	Close(sockfd[1]);			/* close the end we don't use */

	//调用waitpid等待子进程终止
	//子进程的终止状态在status中返回
	Waitpid(childpid, &status, 0);
	//检查该程序是否正常终止（也就是说未被某个信号终止）
	if (WIFEXITED(status) == 0)
		err_quit("child did not terminate");
	//若正常终止，调用WEXITSTATUS宏把终止状态换成“退出状态”
	//“退出状态”的取值在0~255之间
	if ( (status = WEXITSTATUS(status)) == 0)
		//调用read_fd函数通过流管道接收描述符
		//除了描述符外，还读取1个字节的数据，但不对数据进行任何处理
		//“还读取1个字节的数据”的原因：通过流管道发送和接收描述符时，总是发送
		//至少1个字节的数据。要是不这么做，接收进程将难以辨别read_fd的返回值为0
		//意味着“没有数据（但可能伴有一个描述符）”还是“文件已结束”
		//Read_fd函数的前3个参数和read函数一样，第4个参数是指向某个整数的指针
		//用以返回收取的描述符
		Read_fd(sockfd[0], &c, 1, &fd);
	else {
		//如果openfile程序在打开所请求文件时碰到一个错误，
		//它将以相应的errno值作为退出状态终止自身
		errno = status;		/* set errno value from child's status */
		fd = -1;
	}

	Close(sockfd[0]);
	return(fd);
}
```

**read_fd函数**:

本函数必须处理两个版本的recvmsg：

- 使用msg_control成员
- 使用msg_accrights成员（较老版本）

如果所支持的是msg_control版本，config.h头文件就会定义常量`HAVE_MSGHDR_MSG_CONTROL`

```c
// 源码： lib/read_fd.c

/* include read_fd */
#include	"unp.h"

//该函数的前3个参数和read函数一样，第4个参数指向某个整数的指针，用以返回收到的描述符
ssize_t
read_fd(int fd, void *ptr, size_t nbytes, int *recvfd)
{
	struct msghdr	msg;
	struct iovec	iov[1];
	ssize_t			n;

#ifdef	HAVE_MSGHDR_MSG_CONTROL
	//msg_control缓冲区必须为cmsghdr结构适当地对齐
	//声明了由一个cmsghdr结构和一个字符组构成的一个联合，
	//这个联合确保字符组正确对齐
	//保证对齐的另一个方法是调用malloc，但需要在返回时释放空间
	union {
	  struct cmsghdr	cm;
	  char				control[CMSG_SPACE(sizeof(int))];
	} control_un;
	struct cmsghdr	*cmptr;

	msg.msg_control = control_un.control;
	msg.msg_controllen = sizeof(control_un.control);
#else
	int				newfd;

	msg.msg_accrights = (caddr_t) &newfd;
	msg.msg_accrightslen = sizeof(int);
#endif

	msg.msg_name = NULL;
	msg.msg_namelen = 0;

	iov[0].iov_base = ptr;
	iov[0].iov_len = nbytes;
	msg.msg_iov = iov;
	msg.msg_iovlen = 1;

	if ( (n = recvmsg(fd, &msg, 0)) <= 0)
		return(n);

#ifdef	HAVE_MSGHDR_MSG_CONTROL
	//如果返回了辅助数据，需要验证辅助数据的长度、级别和类型
	if ( (cmptr = CMSG_FIRSTHDR(&msg)) != NULL &&
	    cmptr->cmsg_len == CMSG_LEN(sizeof(int))) {
		if (cmptr->cmsg_level != SOL_SOCKET)
			err_quit("control level != SOL_SOCKET");
		if (cmptr->cmsg_type != SCM_RIGHTS)
			err_quit("control type != SCM_RIGHTS");
		//从中取出新建的描述符，并通过调用者给出的recvfd指针返回该描述符
		//CMSG_DATA返回一个unsigned char指针，指向辅助数据对象的cmsg_data成员
		//我们把它类型强制转换成一个int指针，并取出它指向的整数描述符
		*recvfd = *((int *) CMSG_DATA(cmptr));
	} else
		*recvfd = -1;		/* descriptor was not passed */
#else
/* *INDENT-OFF* */
	if (msg.msg_accrightslen == sizeof(int))
		*recvfd = newfd;
	else
		*recvfd = -1;		/* descriptor was not passed */
/* *INDENT-ON* */
#endif

	return(n);
}
/* end read_fd */

ssize_t
Read_fd(int fd, void *ptr, size_t nbytes, int *recvfd)
{
	ssize_t		n;

	if ( (n = read_fd(fd, ptr, nbytes, recvfd)) < 0)
		err_sys("read_fd error");

	return(n);
}
```

**openfile程序**：

它取出三个必须传入的命令行参数（a.待打开文件的路径名；b.打开方式；c.流管道本的一端对应的描述符号），并调用通常的open函数

- argv[0]:openfile
- argv[1]:argsockfd（流管道本的一端对应的描述符号）
- argv[2]:pathname（待打开文件的路径名）
- argv[3]:argmode（打开方式）

```c
// 源码： unixdomain/openfile.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int		fd;

	if (argc != 4)
		err_quit("openfile <sockfd#> <filename> <mode>");

	//命令行参数中的打开方式两个由my_open格式化为字符串，
	//需要使用atoi把它们转回整数
	//argv[2]为待打开文件的路径名；argv[3]为打开方式
	if ( (fd = open(argv[2], atoi(argv[3]))) < 0)
		//如果出错，与open对应的errno值就作为进程退出状态的返回
		exit( (errno > 0) ? errno : 255 );

	//argv[1]为流管道的一端对应的描述符
	//将描述符传递回父进程
	if (write_fd(atoi(argv[1]), "", 1, fd) < 0)
		exit( (errno > 0) ? errno : 255 );

	//write_fd把描述符传递回父进程之后，本进程立即终止
	//本章之前说过：发送进程可以不等落地就关闭已传递的描述符（调用exit时发生）
	//因为内核知道该描述符在飞行中，从而为接收进程保持其打开状态
	exit(0);
}
```

**write_fd函数**：

调用sendmsg跨一个Unix域套接字发送一个描述符

```c
// 源码： lib/write_fd.c

/* include write_fd */
#include	"unp.h"

ssize_t
write_fd(int fd, void *ptr, size_t nbytes, int sendfd)
{
	struct msghdr	msg;
	struct iovec	iov[1];

#ifdef	HAVE_MSGHDR_MSG_CONTROL
	union {
	  struct cmsghdr	cm;
	  char				control[CMSG_SPACE(sizeof(int))];
	} control_un;
	struct cmsghdr	*cmptr;

	msg.msg_control = control_un.control;
	msg.msg_controllen = sizeof(control_un.control);

	cmptr = CMSG_FIRSTHDR(&msg);
	cmptr->cmsg_len = CMSG_LEN(sizeof(int));
	cmptr->cmsg_level = SOL_SOCKET;
	cmptr->cmsg_type = SCM_RIGHTS;
	*((int *) CMSG_DATA(cmptr)) = sendfd;
#else
	msg.msg_accrights = (caddr_t) &sendfd;
	msg.msg_accrightslen = sizeof(int);
#endif

	msg.msg_name = NULL;
	msg.msg_namelen = 0;

	iov[0].iov_base = ptr;
	iov[0].iov_len = nbytes;
	msg.msg_iov = iov;
	msg.msg_iovlen = 1;

	return(sendmsg(fd, &msg, 0));
}
/* end write_fd */

ssize_t
Write_fd(int fd, void *ptr, size_t nbytes, int sendfd)
{
	ssize_t		n;

	if ( (n = write_fd(fd, ptr, nbytes, sendfd)) < 0)
		err_sys("write_fd error");

	return(n);
}
```

# 第16章 非阻塞式I/O

套接字的默认状态是**阻塞**的，当发出一个不能立即完成的套接字调用时，其进程被投入睡觉，等待相应操作完成

**非阻塞式IO**：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_6_2_%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8FIO%E6%A8%A1%E5%9E%8B.png)

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

如果对于一个非阻塞的TCP套接字调用connect，并且连接不能立即建立，那么照样会发起连接，但会返回一个EINPROGRESS错误。接着使用select检查这个连接或成功或失败的已建立条件

非阻塞的connect有**3个用途**：

- 1.**可以把3路握手叠加在其他处理上**（完成一个connect要花一个RTT，而RTT波动范围很大，这段时间内也许有想要执行的其他处理工作可执行） 
- 2.**同时建立多个连接**
- 3.既然使用select等待连接建立，**可以给select指定一个时间限制，缩短connect的超时**（许多实现有着75s到数分钟的connect超时时间） 

使用非阻塞式connect时，**必须处理下列细节**：

- 1.尽管套接字非阻塞，如果连接到的服务器在同一个主机上，那么当我们调用connect时，连接通常立刻建立。因此，必须处理这种情况
- 2.源自Berkeley的实现对于select和非阻塞connect有以下两个规则：
  - 当连接成功建立时，描述符变为可写
  - 当连接建立遇到错误时，描述符变为既可读又可写

## 16.4 非阻塞connect：时间获取客户程序

**main函数**：

```c
// 源码： nonblock/daytimetcpcli.c

#include	"unp.h"

int
main(int argc, char **argv)
{
	int					sockfd, n;
	struct sockaddr_in	servaddr;
	char				recvline[MAXLINE + 1];

	if ( (sockfd = socket(PF_INET, SOCK_STREAM, 0)) < 0)
		err_sys("socket error");

	bzero(&servaddr, sizeof(servaddr));
	servaddr.sin_family      = AF_INET;
	servaddr.sin_addr.s_addr = inet_addr(argv[1]);
	servaddr.sin_port        = htons(13);	/* daytime server */

	//调用非阻塞式connect
	if (connect_nonb(sockfd, (SA *) &servaddr, sizeof(servaddr), 0) < 0)
		err_sys("connect error");

	for ( ; ; ) {
		if ( (n = read(sockfd, recvline, MAXLINE)) <= 0) {
			if (n == 0)
				break;		/* server closed connection */
			else
				err_sys("read error");
		}
		recvline[n] = 0;	/* null terminate */
		Fputs(recvline, stdout);
	}
	exit(0);
}
```

**connect_nonb函数**：

前3个参数与connect相同，第4个参数是等待连接完成的秒数。值为0暗指不给select设置超时；因此内核将使用通常的TCP连接建立超时

```c
// 源码： lib/connect_nonb.c

#include	"unp.h"

int
connect_nonb(int sockfd, const SA *saptr, socklen_t salen, int nsec)
{
	int				flags, n, error;
	socklen_t		len;
	fd_set			rset, wset;
	struct timeval	tval;

	flags = Fcntl(sockfd, F_GETFL, 0);
	Fcntl(sockfd, F_SETFL, flags | O_NONBLOCK);

	error = 0;
	//期待的错误是EINPROGRESS
	//connect返回的任何其他错误返回给函数的调用者
	if ( (n = connect(sockfd, saptr, salen)) < 0)
		if (errno != EINPROGRESS)
			return(-1);

	/* Do whatever we want while the connect is taking place. */

	//如果非阻塞返回0，那么连接已经建立
	//即服务器处于客户所在主机时可能发生这种情况
	if (n == 0)
		goto done;	/* connect completed immediately */

	FD_ZERO(&rset);
	FD_SET(sockfd, &rset);
	wset = rset;
	tval.tv_sec = nsec;
	tval.tv_usec = 0;

	//如果调用者将nsec设为0，则需要将select的最后一个参数设为NULL
	//表示使用默认超时时间
	if ( (n = Select(sockfd+1, &rset, &wset, NULL,
					 nsec ? &tval : NULL)) == 0) {
		close(sockfd);		/* timeout */
		errno = ETIMEDOUT;
		return(-1);
	}

	//如果描述符变为可读或可写
	if (FD_ISSET(sockfd, &rset) || FD_ISSET(sockfd, &wset)) {
		len = sizeof(error);
		//调用getsockopt获取待处理错误
		//如果连接建立成功，该值为0，则不做处理
		//如果连接建立发生错误，该值就是对应连接错误的errno
		if (getsockopt(sockfd, SOL_SOCKET, SO_ERROR, &error, &len) < 0)
			return(-1);			/* Solaris pending error */
	} else
		err_quit("select error: sockfd not set");

done:
	//恢复套接字的文件状态标志
	Fcntl(sockfd, F_SETFL, flags);	/* restore file status flags */
	
	//如果自getsockopt返回的errno变量为非0值，
	//把该值存入errno，函数本身返回-1
	if (error) {
		close(sockfd);		/* just in case */
		errno = error;
		return(-1);
	}
	return(0);
}
```

**移植性问题**：

套接字的各种实现以及非阻塞connect会带来移植性问题。非阻塞connect是网络编程中最不易移植的部分（比如，一个移植性问题是如何判断连接成功建立，具体参考P353）

非阻塞connect不可移植，不同的实现有不同的手段指示连接已经成功建立或已经碰到错误

**被中断的connect**：

被中断的connect：对于一个正常的阻塞式套接字，如果其上的connect调用在TCP三路握手完成前被中断（譬如捕获了某个信号）：如果connect调用不由内核自动重启，那么它将返回EINTR。不能再次调用connect等待未完成的连接继续完成，否则会返回EADDRINUSE错误。只能调用select像处理非阻塞式connect那样处理

## 16.5 非阻塞connect：Web客户程序

客户先建立一个与某个Web服务器的HTTP连接，再获取一个主页。该主页往往含有多个对于其他网页的引用。客户可以使用非阻塞connect同时获取多个网页。以此取代每次只获取一个网页的串行获取手段

在处理Web客户时，第一个连接独立执行，来自该连接的数据含有多个引用，随后用于访问这些引用的多个连接则并行执行

执行本程序的一个典型例子如下：

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_16_13_%E5%90%8E_Web%E7%A8%8B%E5%BA%8F%E6%89%A7%E8%A1%8C%E4%BE%8B%E5%AD%90.png)

**解释**：

- `3`：执行最多3个连接

- `/`：服务器的根网页
- `image*.gif`：一个GIF图像文件

**web.h头文件**：

```c
// 源码： nonblock/web.h

#include	"unp.h"

//本程序最多度MAXFILES个来自Web服务器的文件
#define	MAXFILES	20
#define	SERV		"80"	/* port number or service name */

struct file {
  char	*f_name;			/* 文件名（复制自命令行参数） */
  char	*f_host;			/* 文件所在服务器主机名或IP名 */
  int    f_fd;				/* 用于读取文件的套接字描述符 */
  int	 f_flags;			/* 用于指定准备对文件执行什么操作（连接、读取或完成）的一组标志*/
} file[MAXFILES];

#define	F_CONNECTING	1	/* connect() in progress */
#define	F_READING		2	/* connect() complete; now reading */
#define	F_DONE			4	/* all done */

#define	GET_CMD		"GET %s HTTP/1.0\r\n\r\n"

			/* globals */
int		nconn, nfiles, nlefttoconn, nlefttoread, maxfd;
fd_set	rset, wset;

			/* function prototypes */
void	home_page(const char *, const char *);
void	start_connect(struct file *);
void	write_get_cmd(struct file *);
```

**web main函数**：

```c
// 源码： nonblock/web.c

/* include web1 */
#include	"web.h"

int
main(int argc, char **argv)
{
	int		i, fd, n, maxnconn, flags, error;
	char	buf[MAXLINE];
	fd_set	rs, ws;

	if (argc < 5)
		err_quit("usage: web <#conns> <hostname> <homepage> <file1> ...");
	//获取最多执行连接数
	maxnconn = atoi(argv[1]);

	//根据命令行参数的相关信息填写file结构数组
	nfiles = min(argc - 4, MAXFILES);
	for (i = 0; i < nfiles; i++) {
		file[i].f_name = argv[i + 4];
		file[i].f_host = argv[2];
		file[i].f_flags = 0;
	}
	printf("nfiles = %d\n", nfiles);

	//创建一个TCP连接，发出一个命令到服务器，读取主页
	//第一个连接，需要在我们开始并行建立多个连接之前独自完成
	home_page(argv[2], argv[3]);

	FD_ZERO(&rset);
	FD_ZERO(&wset);
	maxfd = -1;
	//nlefttoread是待读取的文件数（当它达到0时程序任务完成）
	//nlefttoconn是尚无TCP连接的文件数
	//nconn是当前打开着的连接数（它不能超过命令行参数[1]）
	nlefttoread = nlefttoconn = nfiles;
	nconn = 0;
/* end web1 */
/* include web2 */
	//主循环：
	//只要还有文件要处理（nlefttoread大于0）
	while (nlefttoread > 0) {
		//如果没有达到最大并行连接数且另有连接需要建立
		//那就找到一个尚未处理的文件（由值为0的f_flags指示）
		//然后调用start_connect发起另一连接
		while (nconn < maxnconn && nlefttoconn > 0) {
				/* 4find a file to read */
			for (i = 0 ; i < nfiles; i++)
				if (file[i].f_flags == 0)
					break;
			if (i == nfiles)
				err_quit("nlefttoconn = %d but nothing found", nlefttoconn);
			//发起非阻塞connect
			start_connect(&file[i]);
			nconn++;
			nlefttoconn--;
		}

		rs = rset;
		ws = wset;
		//在所有活跃的描述符上使用select，
		//以便处理非阻塞连接的建立，又处理来自服务器的数据
		//一个非阻塞connect正在进展的描述符可能会同时开启读写
		//连接建立完毕并正在等待来自服务器的数据的描述符只会开启读
		n = Select(maxfd+1, &rs, &ws, NULL, NULL);

		//遍历files结构数组中的元素，确定哪些描述符需要处理
		for (i = 0; i < nfiles; i++) {
			flags = file[i].f_flags;
			//flags为0表示尚未开始处理
			//flags为F_DONE表示处理完成
			//对于这两种flags直接跳过
			if (flags == 0 || flags & F_DONE)
				continue;
			fd = file[i].f_fd;
			//对于设置了F_CONNECTING标志的一个描述符
			//如果它在读描述符集或写描述符集中对应的位已经打开
			//那么非阻塞connect已经完成
			//调用getsockopt获取该套接字的待处理错误，如果为0，那么连接成功
			if (flags & F_CONNECTING &&
				(FD_ISSET(fd, &rs) || FD_ISSET(fd, &ws))) {
				n = sizeof(error);
				if (getsockopt(fd, SOL_SOCKET, SO_ERROR, &error, &n) < 0 ||
					error != 0) {
					err_ret("nonblocking connect failed for %s",
							file[i].f_name);
				}
					/* 4connection established */
				printf("connection established for %s\n", file[i].f_name);
				//connect成功的话，关闭该描述符在写描述符集中对应的位
				//并向服务器发送HTTP请求到服务器
				FD_CLR(fd, &wset);		/* no more writeability test */
				write_get_cmd(&file[i]);/* write() the GET command */

			} else if (flags & F_READING && FD_ISSET(fd, &rs)) {
				//对于设置了F_READING的描述符，且它读就绪
				//调用read获取数据
				if ( (n = Read(fd, buf, sizeof(buf))) == 0) {
					//如果read返回0，表示收到服务器的FIN
					//关闭套接字，并设置F_DONE标志
					//关闭该套接字在读描述符集中对应的位
					printf("end-of-file on %s\n", file[i].f_name);
					Close(fd);
					file[i].f_flags = F_DONE;	/* clears F_READING */
					FD_CLR(fd, &rset);
					nconn--;
					nlefttoread--;
				} else {
					printf("read %d bytes from %s\n", n, file[i].f_name);
				}
			}
		}
	}
	exit(0);
}
/* end web2 */
```

**home_page函数**：

```c
// 源码： nonblock/home_page.c

#include	"web.h"

void
home_page(const char *host, const char *fname)
{
	int		fd, n;
	char	line[MAXLINE];

	//建立一个与服务器的连接
	//阻塞式connect
	fd = Tcp_connect(host, SERV);	/* blocking connect() */

	//发出一个HTTP GET命令以获取主页（文件名经常是/）
	n = snprintf(line, sizeof(line), GET_CMD, fname);
	Writen(fd, line, n);

	for ( ; ; ) {
		//读取应答（不对应答做任何操作）
		if ( (n = Read(fd, line, MAXLINE)) == 0)
			break;		/* server closed connection */

		printf("read %d bytes of home page\n", n);
		/* do whatever with data */
	}
	printf("end-of-file on home page\n");
	//关闭连接
	Close(fd);
}

```

**start_connect函数**：

```c
#include	"web.h"

void
start_connect(struct file *fptr)
{
	int				fd, flags, n;
	struct addrinfo	*ai;

	//Host_serv查找并转换主机名和服务名
	//它返回指向某个addrinfo结构数组的一个指针
	//只使用其中第一个结构
	ai = Host_serv(fptr->f_host, SERV, 0, SOCK_STREAM);

	//创建一个TCP套接字
	fd = Socket(ai->ai_family, ai->ai_socktype, ai->ai_protocol);
	fptr->f_fd = fd;
	printf("start_connect for %s, fd %d\n", fptr->f_name, fd);

		/* 4Set socket nonblocking */
	//把该套接字设置为非阻塞
	flags = Fcntl(fd, F_GETFL, 0);
	Fcntl(fd, F_SETFL, flags | O_NONBLOCK);

		/* 4Initiate nonblocking connect to the server. */
	//发起非阻塞connect
	if ( (n = connect(fd, ai->ai_addr, ai->ai_addrlen)) < 0) {
		if (errno != EINPROGRESS)
			err_sys("nonblocking connect error");
		//如果没有立马连接成功
		//把相应文件的标志设为“正在连接”F_CONNECTING
		fptr->f_flags = F_CONNECTING;
		//设置该套接字在读描述符集合写描述符集中对应的位
		//因为select将等待其中任何一个条件变为真，作为连接已建立完毕的指示
		FD_SET(fd, &rset);			/* select for reading and writing */
		FD_SET(fd, &wset);
		if (fd > maxfd)
			maxfd = fd;

	} else if (n >= 0)				/* connect is already done */
		//如果connect立即成功返回
		//表示连接已经建立
		//调用write_get_cmd函数发送一个命令到服务器
		write_get_cmd(fptr);	/* write() the GET command */

	//connet把套接字设为非阻塞后，不再把它重置为默认的阻塞模式
	//这么做没有问题，因为我们只往套接字中写入少量的数据，
	//写入的数据比发送缓冲区小很多，因此可是说是不会阻塞的
}
```

**write_get_cmd函数**：

```c
// 源码： nonblock/write_get_cmd.c

#include	"web.h"

void
write_get_cmd(struct file *fptr)
{
	int		n;
	char	line[MAXLINE];

	//构造命令
	n = snprintf(line, sizeof(line), GET_CMD, fptr->f_name);
	//将命令写到套接字中
	Writen(fptr->f_fd, line, n);
	printf("wrote %d bytes for %s\n", n, fptr->f_name);

	//设置F_READING，它同时清除F_CONNECTING标志
	//该命令向main函数主循环指出：本描述符已准备好提供输入
	fptr->f_flags = F_READING;			/* clears F_CONNECTING */

	//在读描述符集中打开与本描述符对应的位
	FD_SET(fptr->f_fd, &rset);			/* will read server's reply */
	if (fptr->f_fd > maxfd)
		maxfd = fptr->f_fd;
}
```

**优化**：

- 1.当select告知已经就绪的那么多描述符被处理完之后，可以终止main函数中select之后的for循环
- 2.如果可能的话，可以减小maxfd的值，省得select检查那些不再设置的描述符位

**同时连接的性能**：如果网络中存在拥塞，这个技术就会有缺陷。当一个客户到一个服务器建立多个连接时，这些连接之间在TCP层并无通信。即使其中一个连接遇到分组丢失（隐式指示网络已经拥塞），IP地址对相同的其他连接也不会得到通知

## 16.6 非阻塞accpet

**背景**：一个繁忙的服务器，它无法在select返回监听套接字的可读条件后马上调用accept。当客户在服务器调用select后，accept之前中止某个连接时（客户向服务器发送RST），源自Berkeley的实现不把这个中止的连接返回给服务器，而其他实现本应该返回ECONNABORTED错误，却返回的是EPROTO错误

**问题**：在服务器从select返回到调用accept期间，服务器TCP收到来自客户的RST。这个已完成的连接被服务器TCP驱逐出队列（假设其中没有其他已完成的连接），服务器调用accept，但是由于没有任何已经完成的连接，服务器于是阻塞。从而服务器单纯地阻塞在accept调用上，无法处理任何其他就绪的描述符

**解决方法**：

- 1.当使用select获悉某个监听套接字上何时有已完成连接准备好被accept时，总是把监听套接字设为**非阻塞**
- 2.在后续的accpet调用中忽略以下错误：
  - EWOULDBLOCK（Berkeley的实现，客户中止连接时）
  - ECONNABORTED（POSIX实现，客户中止连接时）
  - EPROTO（SVR4实现，客户中止连接时）
  - EINTR（如果有信号被捕获）

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

[Pthread创建线程后必须使用join或detach释放线程资源](http://www.cppblog.com/prayer/archive/2012/04/23/172427.html)

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

**背景知识**：一个线程可以是**可汇合的**（joinable，默认值），也可以是**脱离的**（detached）。当一个可汇合的线程终止时，它的线程ID和退出状态将留存到另一个线程对它调用`pthread_join`。脱离的线程却像守护进程，当它终止时，所有相关资源都被释放，我们不能等待它们终止。`pthread_detach`之后就不能用`pthread_join`了，否则会出现Invalid argument错误 

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

- status：指针status不能指向局部于调用线程的对象，因为线程终止时这样的对象也消失

如果本线程未曾脱离，它的线程ID和退出状态将一直留存到调用进程内的某个其他线程对它调用pthread_join

**让一个线程终止的其他两个方法**：

- 1.启动线程的函数（即pthread_create的第三个参数fun）可以返回。既然该函数（fun）必须声明成返回一个void指针，它的返回值就是相应线程的终止状态
- 2.如果进程的main函数返回或者任何线程调用了exit，整个进程就终止，其中包括它的任何线程

## 26.3 使用线程的str_cli函数（客户）

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_1_%E4%BD%BF%E7%94%A8%E7%BA%BF%E7%A8%8B%E9%87%8D%E6%96%B0%E7%BC%96%E5%86%99str_cli.png)

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
	//如果服务器过早终止，则尚未读入EOF的该线程将由main函数调用exit终止（str_cli返回到main）
	return(NULL);
		/* 4return (i.e., thread terminates) when EOF on stdin */
}
```

**新线程需要的参数传递方法**：

- 1.如代码中所示，使用外部变量
- 2.把这两个值放到一个结构中，然后把指针指向这个结构的一个指针作为参数arg传递给我们要创建的线程

**性能分析**：该版本略快于使用fork的版本，不过慢于非阻塞式I/O的版本。但非阻塞式I/O版本的复杂度和线程版本的简单性，我们依然推荐使用线程而不是非阻塞式I/O

## 26.4 使用线程的TCP回射服务器程序（main）

该版本的TCP回射服务器程序为每个客户使用一个线程，而不是为每个客户使用一个进程

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

主线程中只有一个整数变量connfd，每次调用accept该变量都会被覆写一个新值（已连接描述符）。问题出现在多个线程不是同步地访问一个共享变量（以取得存放在connfd中的整数值）

**解决方法**：

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

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_5_%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E5%87%BD%E6%95%B0.png)

让一个函数线程安全的共通技巧是定义一个名字以_r结尾的新函数。其中两个函数（ctermid和tmpnam）的线程安全条件是：调用者为返回这预先分配空间，并把指向该空间的指针作为参数传递给函数

## 26.5 线程特定数据

**问题**：把一个未线程化的程序转换成使用线程的版本时，有时会碰到因其中有函数使用静态变量而引起的一个常见编程错误。这个错误造成的故障也是非确定的。当一个进程内的不同线程（信号处理函数也视为线程）几乎同时调用这样的函数时就可能会有问题发生，因为这些函数使用的静态变量无法为不同的线程保存各自的值

**解决方法**：

- 1.使用**线程特定数据**
  - 优点：调用顺序无需改变，所有变动都体现在库函数中而非调用这些函数的应用程序中
  - 缺点：这个办法并不简单，而且转换成了**只能**在支持线程上的系统上工作的函数
- 2.改变调用顺序
  - 做法：由调用者把这类函数的所有调用**参数**都封装在一个结构中，并在该结构中存入用到的静态变量
  - 优点：支持线程和不支持线程的系统都可以使用
  - 缺点：调用该函数的应用程序必须修改
- 3.改变接口的结构，避免使用静态变量，这样函数就可以是线程安全的

使用**线程特定数据**是使得现有函数变为线程安全的一个常用技巧，下面对这一知识点进行讲解

需要注意区分**两种结构**：

- 系统为每个进程维护的**key结构**
- 系统为进程内每个线程维护的**Pthread结构**，其部分内容为**pkey数组**

> **key结构**

每个系统支持优先数量的线程特定数据元素，POSIX要求这个限制不小于128（每个进程）。系统（可能是线程函数库）为每个进程维护一个我们称之为**key结构**的结构数组

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_7_%E7%BA%BF%E7%A8%8B%E7%89%B9%E5%AE%9A%E6%95%B0%E6%8D%AE%E7%9A%84%E5%8F%AF%E8%83%BD%E5%AE%9E%E7%8E%B0.png)

- **标志**：指示这个数组元素是否正在使用，初始化为“不在使用”。当一个线程调用`pthread_key_create(pthread_key_t *keyptr,void (*destructor)(void *value))`创建一个新的线程特定数据元素时，系统搜索其key结构数组找出第一个不在使用的元素。该元素的索引（0~127）称为键，返回给调用线程的正是这个引用(通过第一个参数keyptr返回)。
- **析构函数指针**：调用`pthread_key_create`函数时，不仅返回了**键**，还在**key结构**中插入了析构线程特定数据的**析构函数的指针**

> **Pthread结构和pkey数组**

系统还在进程内维护关于每个线程的多条信息，称之为**Pthread结构**，其部分内容是我们称之为**pkey数组**的一个128元素的指针数组。**pkey数组**的所有元素被初始化为空指针。**pkey数组**中的128个指针是和**key结构**中的128个可能的“键”逐一关联的值

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_8_%E7%B3%BB%E7%BB%9F%E7%BB%B4%E6%8A%A4%E7%9A%84%E5%85%B3%E4%BA%8E%E6%AF%8F%E4%B8%AA%E7%BA%BF%E7%A8%8B%E7%9A%84%E4%BF%A1%E6%81%AF.png)

当我们调用pthread_key_create创建一个键时，系统告诉我们这个**键**（根据**key结构**中第一个“不在使用”元素获得，并在该元素中插入了**析构函数**指针）。每个线程根据系统告知的**键**在自己的**pkey数组**中获得相应的指针，并通过调用malloc为该指针初始化。初始化后**pkey数组**中的指针指向的内存，就是真正的**线程特定数据**

**使用线程特定数组的例子**（以调用readline函数为例）：

- 1.一个进程被启动，多个线程被创建
- 2.其中一个线程（如线程0）是首个调用readline函数的线程，该函数转而调用pthread_key_create。系统在**key结构**中找到第一个未使用的元素，并把它的键（0~127）返回给调用者，本例中假设键为1。同时插入析构函数指针
  - 这其中涉及一个名为pthread_once的函数，该函数确保pthread_key_create只被调用一次（第一个调用readline的线程调用）
- 3.readline调用pthread_getspecific获取本线程的**pkey[1]**值，若之前本线程没有调用过readline函数，则**pkey[1]**的值为空指针（否则，返回一个指向由malloc分配的内容的指针）。于是readline调用malloc分配内存，用于为本线程跨相继的readline调用保存特定于线程的信息（即旧版本中的静态数据）。readline函数初始化该内存区，并调用pthread_setspecific把对应所创建键的线程特定数据指针（**pkey[1]**）设置为指向它刚刚分配的内存区（Pthread结构由系统维护，malloc的内存区域由readline函数维护）

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_9_%E6%8A%8Amalloc%E5%88%B0%E7%9A%84%E5%86%85%E5%AD%98%E5%8C%BA%E5%92%8C%E7%BA%BF%E7%A8%8B%E7%89%B9%E5%AE%9A%E6%95%B0%E6%8D%AE%E6%8C%87%E9%92%88%E7%9B%B8%E5%85%B3%E8%81%94.png)

- 4.另一个线程（如线程n）调用readline，当时也许线程0仍然在readline内执行。readline调用pthread_once试图由其调用pthread_key_create初始化它的线程关于readline函数的线程特定数据元素所用的键，但在线程1时已经将readline函数的键初始化为1了，因此就不再被调用
- 5.readline调用pthread_getspecific获取本线程的pkey[1]值，返回值是一个空指针。线程n于是就像线程0那样先调用malloc，再调用pthread_setspecific，以初始化pkey[1]所指向的内容

![](../../pics/network/unp%E7%AC%94%E8%AE%B0/Pic_26_10_%E7%BA%BF%E7%A8%8Bn%E5%88%9D%E5%A7%8B%E5%8C%96%E5%AE%83%E7%9A%84%E7%BA%BF%E7%A8%8B%E7%89%B9%E5%AE%9A%E6%95%B0%E6%8D%AE%E5%90%8E%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84.png)

- 6.当一个线程终止时会发生什么？这就是**key结构**中的**析构函数指针**的用武之地。一个线程调用pthread_key_create创建某个线程特定数据元素时，所指定的函数参数之一就是指向某个析构函数的指针。当一个线程终止时，系统将扫描该线程的**pkey数组**，为每个非空的pkey指针调用相应的析构函数（在**key结构**中获得）

下面介绍**相关函数**

```c
#include <pthread.h>

/*
** 作用：根据onceptr所指向的值判断，是否还需要再次调用init所指向的函数，用以确保pthread_key_create函数在一个线程特定数据中只调用一次
** @onceptr：用于判断是否需要再次调用init所指向函数的值
** @init：函数指针
** @返回值：若成功则为0，若出错则为正的Exxx值
*/
int pthread_once(pthread_once_t *onceptr, void (*init)(void));

/*
** 作用：获取key结构中的一个键，存入keyptr中，并将destructor函数指针插入key结构。对于每个线程特定数据，应该只调用一次该函数
** @keyptr：key结构中的一个键
** @destructor：析构函数指针
** @返回值：若成功则为0，若出错则为正的Exxx值
*/
int pthread_key_create(pthead_key_t *keyptr, void (*destructor)(void *value));

/**
** 作用：返回线程Pthread结构中pkey数组key索引中的元素，一个指针
** @key：一个键，或索引
** @返回值：pkey数组中的元素，一个指向线程特定数据的指针（或空指针）
*/
void* pthread_getspecific(pthread_key_t key);

/**
** 作用：根据key找到线程Pthread结构中pkey数组Key索引的元素，将该元素设为value
** @key：一个键，或索引
** @value：一块由malloc分配的内存块，即线程特定数据
** @返回值：若成功则为0，若出错则为正的Exxx值
*/
int pthread_setspecific(phread_key_t key, const void* value);
```

**例子：使用线程特定数据的readline函数**

通过把3.9节中的readline函数的优化版本转换为无需改变调用顺序的线程安全版本

```c
// 源码： threads/readline.c

/* include readline1 */
#include	"unpthread.h"

//存储readline函数使用的键
static pthread_key_t	rl_key;
//用以判断Pthread_key_create是否第一次调用
static pthread_once_t	rl_once = PTHREAD_ONCE_INIT;

static void
readline_destructor(void *ptr)
{
	free(ptr);
}

//由ptherad_once调用，创建readline使用的键
static void
readline_once(void)
{
	Pthread_key_create(&rl_key, readline_destructor);
}

//存储线程特定数据，旧版本中声明为static
//调用readline的每个线程都由readline动态分配一个Rline结构，
//然后由readline_destructor函数析构
typedef struct {
  int	 rl_cnt;			/* initialize to 0 */
  char	*rl_bufptr;			/* initialize to rl_buf */
  char	 rl_buf[MAXLINE];
} Rline;
/* end readline1 */

/* include readline2 */
/**
** @tsd：指向预先为本线程分配的Rline结构（线程特定数据）的一个指针
*/
static ssize_t
my_read(Rline *tsd, int fd, char *ptr)
{
	if (tsd->rl_cnt <= 0) {
again:
		if ( (tsd->rl_cnt = read(fd, tsd->rl_buf, MAXLINE)) < 0) {
			if (errno == EINTR)
				goto again;
			return(-1);
		} else if (tsd->rl_cnt == 0)
			return(0);
		tsd->rl_bufptr = tsd->rl_buf;
	}

	tsd->rl_cnt--;
	*ptr = *tsd->rl_bufptr++;
	return(1);
}

ssize_t
readline(int fd, void *vptr, size_t maxlen)
{
	size_t		n, rc;
	char	c, *ptr;
	Rline	*tsd;
	//调用pthread_once使得本进程内第一个调用readline的线程通过调用
	//Pthread_key_create创建线程特定数据使用的键
	Pthread_once(&rl_once, readline_once);
	//pthread_getspecific返回指向特定于本线程的Rline结构的指针
	//如果是本线程首次调用，返回值为NULL
	//首次调用则分配一个Rline结构的空间，并由calloc将其rl_cnt成员初始化为0
	//再调用Pthread_setspecific为本线程存储这个指针
	//一下次本线程调用readline时，pthread_getspecific可以返回一个非空指针（指向本线程的线程特定数据）
	if ( (tsd = pthread_getspecific(rl_key)) == NULL) {
		tsd = Calloc(1, sizeof(Rline));		/* init to 0 */
		Pthread_setspecific(rl_key, tsd);
	}

	ptr = vptr;
	for (n = 1; n < maxlen; n++) {
		if ( (rc = my_read(tsd, fd, &c)) == 1) {
			*ptr++ = c;
			if (c == '\n')
				break;
		} else if (rc == 0) {
			*ptr = 0;
			return(n - 1);		/* EOF, n - 1 bytes read */
		} else
			return(-1);		/* error, errno set by read() */
	}

	*ptr = 0;
	return(n);
}
/* end readline2 */

ssize_t
Readline(int fd, void *ptr, size_t maxlen)
{
	ssize_t		n;

	if ( (n = readline(fd, ptr, maxlen)) < 0)
		err_sys("readline error");
	return(n);
}
```

## 26.6 Web客户与同时连接（Solaris线程）

把16.5节的Web客户程序例子重新编写成 用线程代替非阻塞connect。改用线程之后，可以让套接字停留在默认的**阻塞模式**，改而为每个连接创建一个线程，每个线程可以阻塞在它的connect调用中。内核（或线程函数库）会转而运行另外某个就绪的线程

**全局变量**：

```c
// 源码： threads/web01.c

/* include web1 */
#include	"unpthread.h"   //Pthread线程
#include	<thread.h>		/* Solaris线程 */

#define	MAXFILES	20
#define	SERV		"80"	/* port number or service name */

struct file {
  char	*f_name;			/* 文件名（复制自命令行参数） */
  char	*f_host;			/* 文件所在服务器主机名或IP名 */
  int    f_fd;				/* 用于读取文件的套接字描述符 */
  int	 f_flags;			/* 用于指定准备对文件执行什么操作（连接、读取或完成）的一组标志 */
  pthread_t	 f_tid;			/* 新增：线程ID */
} file[MAXFILES];
#define	F_CONNECTING	1	/* connect() in progress */
#define	F_READING		2	/* connect() complete; now reading */
#define	F_DONE			4	/* all done */

#define	GET_CMD		"GET %s HTTP/1.0\r\n\r\n"

int		nconn, nfiles, nlefttoconn, nlefttoread;

void	*do_get_read(void *);
void	home_page(const char *, const char *);
void	write_get_cmd(struct file *);
```

**main函数**：

```c
// 源码： threads/web01.c

int
main(int argc, char **argv)
{
	int			i, n, maxnconn;
	pthread_t	tid;
	struct file	*fptr;

	if (argc < 5)
		err_quit("usage: web <#conns> <IPaddr> <homepage> file1 ...");
	maxnconn = atoi(argv[1]);

	nfiles = min(argc - 4, MAXFILES);
	for (i = 0; i < nfiles; i++) {
		file[i].f_name = argv[i + 4];
		file[i].f_host = argv[2];
		file[i].f_flags = 0;
	}
	printf("nfiles = %d\n", nfiles);

	home_page(argv[2], argv[3]);

	nlefttoread = nlefttoconn = nfiles;
	nconn = 0;
/* end web1 */
/* include web2 */
	while (nlefttoread > 0) {
		//如果创建另一个线程的条件（nconn小于maxnconn）能够满足，就创建一个
		while (nconn < maxnconn && nlefttoconn > 0) {
				/* 4find a file to read */
			for (i = 0 ; i < nfiles; i++)
				if (file[i].f_flags == 0)
					break;
			if (i == nfiles)
				err_quit("nlefttoconn = %d but nothing found", nlefttoconn);

			file[i].f_flags = F_CONNECTING;
			//线程的执行函数是do_get_read，传递给它的参数是指向file结构的指针
			Pthread_create(&tid, NULL, &do_get_read, &file[i]);
			file[i].f_tid = tid;
			nconn++;
			nlefttoconn--;
		}

		//通过指定第一个参数为0调用Solaris线程函数thr_join，等待任何一个线程终止
		//Pthread没有提供等待任一线程终止的手段（pthread_join要求显示指定要等待的线程）
		//Pthread解决本问题需要使用条件变量供即将终止的线程通知主线程何时终止
		//thr_join难以移植到所有环境下
		if ( (n = thr_join(0, &tid, (void **) &fptr)) != 0)
			errno = n, err_sys("thr_join error");

		nconn--;
		nlefttoread--;
		printf("thread id %d for %s done\n", tid, fptr->f_name);
	}

	exit(0);
}
/* end web2 */
```

**home_page函数**：

与16.5节的相比，**无改动**

```c
// 源码： threads/web01.c
#include	"web.h"

void
home_page(const char *host, const char *fname)
{
	int		fd, n;
	char	line[MAXLINE];

	//建立一个与服务器的连接
	//阻塞式connect
	fd = Tcp_connect(host, SERV);	/* blocking connect() */

	//发出一个HTTP GET命令以获取主页（文件名经常是/）
	n = snprintf(line, sizeof(line), GET_CMD, fname);
	Writen(fd, line, n);

	for ( ; ; ) {
		//读取应答（不对应答做任何操作）
		if ( (n = Read(fd, line, MAXLINE)) == 0)
			break;		/* server closed connection */

		printf("read %d bytes of home page\n", n);
		/* do whatever with data */
	}
	printf("end-of-file on home page\n");
	//关闭连接
	Close(fd);
}
```

**do_get_read函数**：

```c
// 源码： threads/web01.c

/* include do_get_read */
//功能：建立TCP连接，给服务器发送一个HTTP GET命令
void *
do_get_read(void *vptr)
{
	int					fd, n;
	char				line[MAXLINE];
	struct file			*fptr;

	fptr = (struct file *) vptr;

	//创建一个TCP套接字并建立一个连接，该套接字时通常的阻塞式套接字
	//该线程将阻塞在connect调用，直到连接建立
	fd = Tcp_connect(fptr->f_host, SERV);
	fptr->f_fd = fd;
	printf("do_get_read for %s, fd %d, thread %d\n",
			fptr->f_name, fd, fptr->f_tid);

	//构造HTTP GET命令并把它发送到服务器
	write_get_cmd(fptr);	/* write() the GET command */

		/* 4Read server's reply */
	for ( ; ; ) {
		//读入服务器应答
		if ( (n = Read(fd, line, MAXLINE)) == 0)
			break;		/* server closed connection */

		printf("read %d bytes from %s\n", n, fptr->f_name);
	}
	printf("end-of-file on %s\n", fptr->f_name);
	//关闭连接
	Close(fd);
	//设置该file完成标志
	fptr->f_flags = F_DONE;		/* clears F_READING */

	//返回，从而终止本线程
	return(fptr);		/* terminate thread */
}
/* end do_get_read */
```

**write_get_cmd函数**：

```c
// 源码： threads/web01.c

/* include write_get_cmd */
void
write_get_cmd(struct file *fptr)
{
	int		n;
	char	line[MAXLINE];

	n = snprintf(line, sizeof(line), GET_CMD, fptr->f_name);
	Writen(fptr->f_fd, line, n);
	printf("wrote %d bytes for %s\n", n, fptr->f_name);

	fptr->f_flags = F_READING;			/* clears F_CONNECTING */
}
/* end write_get_cmd */
```

## 26.7 互斥锁

**问题**：多线程更改一个共享变量

**解决方法**：使用一个互斥锁（mutex）保护这个共享变量。访问该变量的前提条件时持有该锁

对于Pthread，**互斥锁**是**类型**为**pthread_mutex_t**的变量

- **上锁和解锁**

```c
#include <pthread.h>

int pthread_mutex_lock(pthread_mutex_t *mptr);
int pthread_mutex_unlock(pthread_mutex_t *mptr);

//返回值：若成功则为0，若出错，则为正的Exxx值
```

- **锁的初始化**
  - **互斥锁变量是静态分配的**：把它初始化为常值PTHREAD_MUTEX_INITIALIZER
  - **在共享内存区中分配一个互斥锁**：通过调用pthread_mutex_init函数在运行时初始化

**使用互斥锁的例子**：

```c
// 源码： threads/example02.c

#include	"unpthread.h"

#define	NLOOP 5000

int				counter;		/* incremented by threads */
pthread_mutex_t	counter_mutex = PTHREAD_MUTEX_INITIALIZER;

void	*doit(void *);

int
main(int argc, char **argv)
{
	pthread_t	tidA, tidB;

	Pthread_create(&tidA, NULL, &doit, NULL);
	Pthread_create(&tidB, NULL, &doit, NULL);

		/* 4wait for both threads to terminate */
	Pthread_join(tidA, NULL);
	Pthread_join(tidB, NULL);

	exit(0);
}

void *
doit(void *vptr)
{
	int		i, val;

	/*
	 * Each thread fetches, prints, and increments the counter NLOOP times.
	 * The value of the counter should increase monotonically.
	 */

	for (i = 0; i < NLOOP; i++) {
		Pthread_mutex_lock(&counter_mutex);

		val = counter;
		printf("%d: %d\n", pthread_self(), val + 1);
		counter = val + 1;

		Pthread_mutex_unlock(&counter_mutex);
	}

	return(NULL);
}
```

## 26.8 条件变量

**条件变量**可以在某个条件发生之前，将进程投入睡眠 

通常是**条件变量**结合**互斥锁**实现这个功能。互斥锁提供互斥机制，条件变量提供信号机制

- 结合使用的原因：“条件”通常是线程之间共享的某个变量的值，允许不同线程设置和测试该变量，因此要求一个与该变量关联的互斥锁

对于Pthread，**条件变量**是**类型**为**pthread_cond_t**的变量

```c
//#include <pthread.h>

//等待cptr指向的条件变量，投入睡眠之前会释放mptr指向的互斥锁，唤醒后会重新获得mptr指向的互斥锁
int pthread_cond_wait(pthread_cond_t *cptr, pthread_mutex_t *mptr);

//允许线程设置一个阻塞时间的限制。如果超时，返回ETIME错误。这个时间值是一个绝对时间，而不是一个时间增量。也就是说abstime参数是函数应该返回时刻的系统时间——从1970年1月1日UTC时间以来的秒数和纳秒数
int pthread_cond_timewait(pthread_cond_t *cptr, pthread_mutex_t *mptr, const struct timespec *abstime);

//唤醒等待cptr指向的条件变量上的单个线程
int pthread_cond_signal(pthread_cond_t *cptr);

//有时候一个线程应该唤醒多个线程，这种情况下它可以调用该函数唤醒在相应条件变量上的所有线程
int pthread_cond_broadcast(pthread_cond_t *cptr);

```

## 26.9 Web客户与同时连接（Pthread线程）

使用Pthread的pthread_join来回收线程

需要改变的地方：

- 1.**全局变量**：增加一个新标志（F_JOINED）和一个条件变量
- 2.**do_get_read函数**：在线程终止之前递增ndone并通知主循环
- 3.**main函数的主循环**

**全局变量**：

```c
// 源码： threads/web03.c

#include	"unpthread.h"
#include	<thread.h>		/* Solaris threads */

#define	MAXFILES	20
#define	SERV		"80"	/* port number or service name */

struct file {
  char	*f_name;			/* filename */
  char	*f_host;			/* hostname or IP address */
  int    f_fd;				/* descriptor */
  int	 f_flags;			/* F_xxx below */
  pthread_t	 f_tid;			/* thread ID */
} file[MAXFILES];
#define	F_CONNECTING	1	/* connect() in progress */
#define	F_READING		2	/* connect() complete; now reading */
#define	F_DONE			4	/* all done */
#define	F_JOINED		8	/* 线程已经被JOIN过 */

#define	GET_CMD		"GET %s HTTP/1.0\r\n\r\n"

int		nconn, nfiles, nlefttoconn, nlefttoread;

//已经终止但还未join的线程数，需要对其join
int				ndone;		/* number of terminated threads */
//对ndone的互斥量
pthread_mutex_t	ndone_mutex = PTHREAD_MUTEX_INITIALIZER;
//条件变量
pthread_cond_t	ndone_cond = PTHREAD_COND_INITIALIZER;
```

**do_get_read函数**：

```c
// 源码： threads/web03.c

void *
do_get_read(void *vptr)
{
	int					fd, n;
	char				line[MAXLINE];
	struct file			*fptr;

	fptr = (struct file *) vptr;

	fd = Tcp_connect(fptr->f_host, SERV);
	fptr->f_fd = fd;
	printf("do_get_read for %s, fd %d, thread %d\n",
			fptr->f_name, fd, fptr->f_tid);

	write_get_cmd(fptr);	/* write() the GET command */

		/* 4Read server's reply */
	for ( ; ; ) {
		if ( (n = Read(fd, line, MAXLINE)) == 0)
			break;		/* server closed connection */

		printf("read %d bytes from %s\n", n, fptr->f_name);
	}
	printf("end-of-file on %s\n", fptr->f_name);
	Close(fd);
	fptr->f_flags = F_DONE;		/* clears F_READING */
	
    //在本线程终止之前递增ndone并使用条件变量通知主线程
	Pthread_mutex_lock(&ndone_mutex);
	ndone++;
	Pthread_cond_signal(&ndone_cond);
	Pthread_mutex_unlock(&ndone_mutex);

	return(fptr);		/* terminate thread */
}
```

**main函数**：

```c
// 源码： threads/web03.c

int
main(int argc, char **argv)
{
	int			i, maxnconn;
	pthread_t	tid;
	struct file	*fptr;

	if (argc < 5)
		err_quit("usage: web <#conns> <IPaddr> <homepage> file1 ...");
	maxnconn = atoi(argv[1]);

	nfiles = min(argc - 4, MAXFILES);
	for (i = 0; i < nfiles; i++) {
		file[i].f_name = argv[i + 4];
		file[i].f_host = argv[2];
		file[i].f_flags = 0;
	}
	printf("nfiles = %d\n", nfiles);

	home_page(argv[2], argv[3]);

	nlefttoread = nlefttoconn = nfiles;
	nconn = 0;
/* include web2 */
	while (nlefttoread > 0) {
		while (nconn < maxnconn && nlefttoconn > 0) {
				/* 4find a file to read */
			for (i = 0 ; i < nfiles; i++)
				if (file[i].f_flags == 0)
					break;
			if (i == nfiles)
				err_quit("nlefttoconn = %d but nothing found", nlefttoconn);

			file[i].f_flags = F_CONNECTING;
			Pthread_create(&tid, NULL, &do_get_read, &file[i]);
			file[i].f_tid = tid;
			nconn++;
			nlefttoconn--;
		}

			/* 4Wait for thread to terminate */
		//当发现某个线程终止时，我们遍历所有file结构找出这个线程（F_DONE状态）
		//对其调用Pthread_join，并设置为F_JOINED
		Pthread_mutex_lock(&ndone_mutex);
		while (ndone == 0)
			Pthread_cond_wait(&ndone_cond, &ndone_mutex);

		for (i = 0; i < nfiles; i++) {
			if (file[i].f_flags & F_DONE) {
				Pthread_join(file[i].f_tid, (void **) &fptr);

				if (&file[i] != fptr)
					err_quit("file[i] != fptr");
				fptr->f_flags = F_JOINED;	/* clears F_DONE */
				ndone--;
				nconn--;
				nlefttoread--;
				printf("thread %d for %s done\n", fptr->f_tid, fptr->f_name);
			}
		}
		Pthread_mutex_unlock(&ndone_mutex);
	}

	exit(0);
}
```

# 第30章 服务/服务器程序设计范式

## 30.1 概述

开发一个Unix服务器程序时，有如下类型的进程控制可供选择：

- 1.迭代服务器（第1章）
  - 缺点：这样的服务器在完成对当前客户的服务之前无法处理已等待服务的新客户
- 2.多进程并发服务器（第5章）：为每个客户调用fork派生一个子进程
- 3.TCP select单进程服务器（第6章）：TCP服务器程序由使用select处理任意多个客户的单个进程构成
- 4.多线程并发服务器（第26章）：服务器程序被改为服务器为每个客户创建一个线程，以取代派生一个进程
- 5.预先派生子进程服务器（第30章）：让服务器在启动阶段调用fork创建一个子进程池。每个客户请求由当前可用子进程池中的某个（闲置）子进程处理
- 6.预先创建线程服务器（第30章）：让服务器在启动阶段创建一个线程池，每个客户由当前可用线程池中的某个（闲置）线程处理

## 30.2 TCP客户程序设计范式

对本书中实现的TCP客户程序的优缺点总结：

- 1.基本的TCP客户程序（第5章），存在两个问题：
  - a).进程在被阻塞以等待用户输入期间，看不到诸如对端关闭连接等网络事件
  - b).以“停-等”模式运作，批处理效率极低
- 2.select版TCP客户程序（第6章）：通过调用select使得进程能够在等待用户输入期间得到网络事件通知，问题：
  - 不能正确地处理批量输入问题
- 3.select shutdown版TCP客户程序（第6章）：使用shutdown函数解决了2.中“不能正确地处理批量输入问题”
- 4.非阻塞I/O客户程序（第16章）
- 5.多进程客户程序（第16章）：使用fork派生一个子进程，并由父进程（或子进程）处理从客户到服务器的数据，由子进程（或父进程）处理从服务器到客户的数据
- 6.多线程客户程序（第26章）：使用两个线程取代5.中的两个进程

**总结**：

- 1.非阻塞式I/O版本是最快的，但其代码复制度高
- 2.两个进程或两个线程的版本相比之下代码简化得多，而运行速度只是稍逊“非阻塞式I/O版本”

## 30.3 TCP测试用客户程序

给出的客户程序用于测试我们的服务器程序的各个变体

```c
// 源码： server/client.c

#include	"unp.h"

#define	MAXN	16384		/* max # bytes to request from server */

int
main(int argc, char **argv)
{
	int		i, j, fd, nchildren, nloops, nbytes;
	pid_t	pid;
	ssize_t	n;
	char	request[MAXLINE], reply[MAXN];

	//要求输入 0.程序名 1.服务器的主机名或IP地址 2.服务器的端口 3.由客户fork的子进程数
	//4.每个子进程发送给服务器的请求数 5.每个请求要求服务器返送的数据字节数
	if (argc != 6)
		err_quit("usage: client <hostname or IPaddr> <port> <#children> "
				 "<#loops/child> <#bytes/request>");

	nchildren = atoi(argv[3]); //3.由客户fork的子进程数
	nloops = atoi(argv[4]); //4.每个子进程发送给服务器的请求数
	nbytes = atoi(argv[5]); //5.每个请求要求服务器返送的数据字节数
	snprintf(request, sizeof(request), "%d\n", nbytes); /* newline at end */

	for (i = 0; i < nchildren; i++) {
		if ( (pid = Fork()) == 0) {		/* child */
			for (j = 0; j < nloops; j++) {
				fd = Tcp_connect(argv[1], argv[2]);

				Write(fd, request, strlen(request));

				if ( (n = Readn(fd, reply, nbytes)) != nbytes)
					err_quit("server returned %d bytes", n);

				Close(fd);		/* TIME_WAIT on client, not server */
			}
			printf("child %d done\n", i);
			exit(0);
		}
		/* parent loops around to fork() again */
	}

	while (wait(NULL) > 0)	/* now parent waits for all children */
		;
	if (errno != ECHILD)
		err_sys("wait error");

	exit(0);
}
```

执行本客户程序的命令如下：

```shell
# client:程序名
# 206.62.226.36：服务器IP
# 8888：服务器端口号
# 5：由客户fork的子进程数
# 500：每个子进程发送给服务器的请求数
# 4000：每个请求要求服务器返送的数据字节数
% client 206.62.226.36 8888 5 500 4000
```

这将建立2500个服务器的TCP连接（5个子进程*每个子进程500个请求），在每个连接上，客户向服务器发送5个字节数（“4000\n”），服务器向客户返回4000字节数据。我们在**两个**不同的主机上针对同一个服务器执行本客户程序，于是总共提供5000个TCP连接，而且任意时刻服务器端最多同时存在10个连接

以此客户程序分别测试后面9个不同的服务器程序设计范式

- 1.TCP迭代服务器程序
- 2.TCP并发服务器程序，每个客户一个子进程
- 3.TCP预先派生子进程服务器程序，accept无上锁保护
- 4.TCP预先派生子进程服务器程序，accpet使用文件上锁保护
- 5.TCP预先派生子进程服务器程序，accpet使用线程上锁保护
- 6.TCP预先派生子进程服务器程序，传递描述符
- 7.TCP并发服务器程序，每个客户一个线程
- 8.TCP预先创建线程服务器程序，每个线程各自accpet
- 9.TCP预先创建线程服务器程序，主线程统一accpet

## 30.4 TCP迭代服务器程序

从进程控制角度看，迭代服务器是最快的，因为它不执行进程控制

## 30.5 TCP并发服务器程序，每个客户一个子进程

**限制**：在此程序中，客户数目的唯一限制是操作系统对以其名义运行服务器的用户ID能够同时拥有多少个子进程的限制

**问题**：每个客户现场fork一个进程比较耗费CPU时间

**main函数**:

```c
// 源码： server/serv01.c

/* include serv01 */
#include	"unp.h"

int
main(int argc, char **argv)
{
	int					listenfd, connfd;
	pid_t				childpid;
	void				sig_chld(int), sig_int(int), web_child(int);
	socklen_t			clilen, addrlen;
	struct sockaddr		*cliaddr;

	if (argc == 2)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 3)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv01 [ <host> ] <port#>");
	cliaddr = Malloc(addrlen);

	//处理子进程终止时发出的SIGCHLD信号
	Signal(SIGCHLD, sig_chld);
	//捕获由键入终端中断产生的SIGINT信号
	//在客户运行完毕之后键入该键以显示服务器程序运行所需的CPU时间
	//sig_int函数的工作就是输出CPU时间
	Signal(SIGINT, sig_int);

	for ( ; ; ) {
		clilen = addrlen;
		if ( (connfd = accept(listenfd, cliaddr, &clilen)) < 0) {
			if (errno == EINTR)
				continue;		/* back to for() */
			else
				err_sys("accept error");
		}

		if ( (childpid = Fork()) == 0) {	/* child process */
			Close(listenfd);	/* close listening socket */
            //调用web_child函数处理用户请求
            //回复客户以客户要求的字节数
			web_child(connfd);	/* process request */
			exit(0);
		}
		Close(connfd);			/* parent closes connected socket */
	}
}
/* end serv01 */
```

**web_child函数**

```c
// 源码： server/web_child.c

#include	"unp.h"

#define	MAXN	16384		/* max # bytes client can request */

void
web_child(int sockfd)
{
	int			ntowrite;
	ssize_t		nread;
	char		line[MAXLINE], result[MAXN];

	for ( ; ; ) {
		if ( (nread = Readline(sockfd, line, MAXLINE)) == 0)
			return;		/* connection closed by other end */

			/* 4line from client specifies #bytes to write back */
		ntowrite = atol(line);
		if ((ntowrite <= 0) || (ntowrite > MAXN))
			err_quit("client request for %d bytes", ntowrite);

		Writen(sockfd, result, ntowrite);
	}
}
```

**sig_int函数**

```c
// 源码： server/serv01.c


/* include sigint */
void
sig_int(int signo)
{
	void	pr_cpu_time(void);
	
    //输出CPU时间
	pr_cpu_time();
	exit(0);
}
/* end sigint */
```

## 30.6 TCP预先派生子进程服务器程序，accept无上锁保护

在服务器启动阶段预先派生一定数量的子进程，当各个客户程序连接到达时，这些子进程立即就能为它们服务

![](G:/OneDrive/Github/Interview-Summary/pics/network/unp%E7%AC%94%E8%AE%B0/Pic_30_8_%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%A2%84%E5%85%88%E6%B4%BE%E7%94%9F%E5%AD%90%E8%BF%9B%E7%A8%8B.png)

**优点**：无须引入父进程执行fork的开销就能处理新到的客户

**缺点**：

- 服务器端：父进程必须在服务器启动阶段猜测需要预先派生多少个子进程。如果某个时刻客户数恰好等于进程总数，那么新到的客户将被忽略，直到至少有一个子进程重新可用。进一步分析，这些客户并未被完全忽略。内核将为每个新到的客户完成三路握手，直到达到相应套接字上listen调用的backlog数为止，然后再服务器调用accpet时把这些已经完成的连接传递给它
- 客户端：客户能觉察到服务器在响应时间上的恶化，因为尽管它的connect调用立即返回，但是它的第一个请求可能是在一段时间之后才被服务器处理

**解决方法**：通过增加代码，服务器应对客户负载的变动，父进程持续监视可用（即闲置）子进程数：

- 一旦该值降到低于某个阈值就派生额外的子进程
- 一旦该值超过另一个阈值就终止一些过剩的子进程

**main函数（无服务器负载变动）**：

```c
// 源码： server/serv02.c

/* include serv02 */
#include	"unp.h"

static int		nchildren;
static pid_t	*pids;

int
main(int argc, char **argv)
{
	int			listenfd, i;
	socklen_t	addrlen;
	void		sig_int(int);
	pid_t		child_make(int, int, int);

	if (argc == 3)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 4)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv02 [ <host> ] <port#> <#children>");
	//需要fork的子进程数
	nchildren = atoi(argv[argc-1]);
	//分配一个存放各个子进程ID的数组，
	//用于父进程即将终止时由main函数终止所有子进程（在sig_int函数中完成）
	pids = Calloc(nchildren, sizeof(pid_t));

	for (i = 0; i < nchildren; i++)
		//生成子进程，并将子进程ID存入数组中
		pids[i] = child_make(i, listenfd, addrlen);	/* parent returns */

	//服务器父进程终止时的信号处理函数
	//该函数完成的事情：
	//1.终止并wait所有子进程
	//2.打印CPU时间
	Signal(SIGINT, sig_int);
	
	//父进程暂停，由子进程进行处理客户的连接和请求
	for ( ; ; )
		pause();	/* everything done by children */
}
/* end serv02 */
```

**child_make函数**：

```c
// 源码： server/child02.c

/* include child_make */
#include	"unp.h"

pid_t
child_make(int i, int listenfd, int addrlen)
{
	pid_t	pid;
	void	child_main(int, int, int);

	if ( (pid = Fork()) > 0)
		//父进程返回到服务器main函数
		return(pid);		/* parent */

	//子进程，不返回，进入child_main函数
	//该函数是个无限循环，进行客户连接和请求的处理
	child_main(i, listenfd, addrlen);	/* never returns */
}
/* end child_make */
```

**child_main函数**：

```c
// 源码： server/child02.c

/* include child_main */
void
child_main(int i, int listenfd, int addrlen)
{
	int				connfd;
	void			web_child(int);
	socklen_t		clilen;
	struct sockaddr	*cliaddr;

	cliaddr = Malloc(addrlen);

	printf("child %ld starting\n", (long) getpid());
	//子进程在这个循环中反复，直到父进程被终止
	//父进程发送SIGTERM信号终止所有子进程
	for ( ; ; ) {
		clilen = addrlen;
		//每个子进程调用accept返回一个已连接套接字
		connfd = Accept(listenfd, cliaddr, &clilen);
		
		//处理客户请求
		web_child(connfd);		/* process the request */
		//关闭“已连接套接字”
		Close(connfd);
	}
}
/* end child_main */
```

**sig_int函数**：

```c++
// 源码： server/serv02.c

/* include sigint */
void
sig_int(int signo)
{
	int		i;
	void	pr_cpu_time(void);

		/* 4terminate all children */
	//给每个子进程发送SIGTERM信号终止它们
	for (i = 0; i < nchildren; i++)
		kill(pids[i], SIGTERM);
	//调用wait汇集所有子进程的资源利用统计
	while (wait(NULL) > 0)		/* wait for all children */
		;
	if (errno != ECHILD)
		err_sys("wait error");
	//打印CPU时间
	pr_cpu_time();
	exit(0);
}
/* end sigint */
```

### 30.6.1 4.4BSD上实现多进程accept同一监听描述符

本章将描述4.4BSD上如何实现多个进程在同一个监听描述符上调用accept

父进程在派生任何子进程之前创建监听套接字，而每次调用fork时，所有描述符也被复制。下图展示了proc结果（每个进程一个）、监听描述符的单个file结构以及单个socket结构之间的关系

![](../../pics/network/unp笔记/Pic_30_13_proc_file和socket这三个结构之间的关系.png)

**描述符**是本进程引用file结构的proc结构中一个数组中某个元素的下标。子进程中一个给定描述符引用的file结构正是父进程中同一个描述符引用的file结构

**file的引用计数**：每个file结构都有一个引用计数。当打开一个文件或套接字时，内核将为之构造一个file结构，并由作为打开操作返回的**描述符**引用，它的引用计数初值为1；以后每当调用fork以派生子进程或对打开操作返回的**描述符**（或其复制品）调用dup以复制描述符时，该file结构的引用计数就递增（每次增1）

**惊群（thundering herd）问题**：服务器进程在程序启动阶段派生N个子进程，它们各自调用accept并因而均被内核投入睡眠。当第一个客户连接到达时，所有N个子进程均被唤醒。这是因为所有N个子进程所用的监听描述符（它们有相同的值）指向同一个socket结构，致使它们在同一个等待通道，即这个socket结构的so_timeo成员上进入睡眠。尽管所有N个子进程均被唤醒，其中只有最先运行的子进程获得那个客户连接，其余N-1个子进程继续回复睡眠，因为它们将发现队列长度为0（因为最先运行的连接早已取走了本来只有一个的连接）。

- **危害**：尽管只有一个子进程将获得连接，所有N个子进程却都被唤醒了。每当仅有一个连接准备好被接受时却唤醒太多进程的做法会导致**性能损失**。为了避免惊群问题，我们不希望有太多的额外子进程一直闲置着
- **例外**：某些Unix内核有一个往往命名为wakeup_one的函数，它只唤醒等待某个事件的多个进程中的一个，而不是所有。但BSD/OS内核没有这样的函数

### 30.6.3 连接在子进程中的分布

在“TCP预先派生子进程服务器程序，accept无上锁保护”中，内核调度算法把各个连接均匀散步到各个子进程

### 30.6.4 select冲突

**select函数的冲突现象**：当多个进程在引用同一个套接字的描述符上调用select时就会发生冲突

- **原因**：在socket结构中为存放本套接字就绪时应该唤醒哪些进程而分配的仅仅是一个进程ID的空间。如果有多个进程在等待同一个套接字，那么内核必须唤醒的是阻塞在select调用中的所有进程，因为它不知道哪些进程受刚变得就绪的这个套接字影响

**经验**：如果有多个进程阻塞在引用同一个实体（如套接字或普通文件，由file结构直接或间接描述）的描述符上，那么最好直接阻塞在如accept之类的函数而不是select之中

## 3.7 TCP预先派生子进程服务器程序，accept使用文件上锁保护

4.4BSD实现允许多个进程在引用同一个监听套接字的描述符上调用accept，然而这种做法也仅仅适用于在内核中实现accept的源自Berkeley的内核

在基于SVR4的Solaris 2.5内核上运行30.6节的服务器程序，客户开始连接到该服务器后不久，某个子进程的accept就会返回EPROTO错误（表示协议有错）

**原因**：SVR4的流实现机制和库函数版本的accept并**非一个原子操作**。Solaris 2.6修复了这个问题，但大多数SVR4实现仍然存在这个问题

**解决方法**：让应用进程在调用accept前后安置某种形式的锁，任意时刻只有一个子进程阻塞在accept调用中，其他子进程则阻塞在试图获取用于保护accept的锁上

本节使用**fcntl函数呈现的POSIX文件上锁功能**

在30.6版本之上的**修改**：

- 1.服务器main函数：添加了`my_lock_init`来初始化锁
- 2.服务器child_main函数：在调用accept之前获取文件锁，在accept返回后释放文件锁

**main函数**：

```c
// 源码： server/serv03.c

#include	"unp.h"

static int		nchildren;
static pid_t	*pids;

int
main(int argc, char **argv)
{
	int			listenfd, i;
	socklen_t	addrlen;
	void		sig_int(int);
	pid_t		child_make(int, int, int);

	if (argc == 3)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 4)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv03 [ <host> ] <port#> <#children>");
	nchildren = atoi(argv[argc-1]);
	pids = Calloc(nchildren, sizeof(pid_t));
	//初始化锁
	my_lock_init("/tmp/lock.XXXXXX"); /* one lock file for all children */
	for (i = 0; i < nchildren; i++)
		pids[i] = child_make(i, listenfd, addrlen);	/* parent returns */

	Signal(SIGINT, sig_int);

	for ( ; ; )
		pause();	/* everything done by children */
}
```

**child_main函数**

```c
// 源码： server/child03.c

void
child_main(int i, int listenfd, int addrlen)
{
	int				connfd;
	void			web_child(int);
	socklen_t		clilen;
	struct sockaddr	*cliaddr;

	cliaddr = Malloc(addrlen);

	printf("child %ld starting\n", (long) getpid());
	for ( ; ; ) {
		clilen = addrlen;
		//获取文件锁
		my_lock_wait();
		connfd = Accept(listenfd, cliaddr, &clilen);
		//释放文件锁
		my_lock_release();

		web_child(connfd);		/* process the request */
		Close(connfd);
	}
}
```

下面对三个与锁相关的函数进行分析：

- 1.`my_lock_init(char*)`:初始化文件锁
- 2.`my_lock_wait()`：获取文件锁
- 3.`my_lock_release()`：释放文件锁

**my_lock_init函数**：

```c
// 源码： server/lock_fcntl.c

/* include my_lock_init */
#include	"unp.h"

static struct flock	lock_it, unlock_it;
static int			lock_fd = -1;
					/* fcntl() will fail if my_lock_init() not called */

//调用者将一个路径名模板指定为my_lock_init的函数参数
void
my_lock_init(char *pathname)
{
    char	lock_file[1024];

		/* 4must copy caller's string, in case it's a constant */
    strncpy(lock_file, pathname, sizeof(lock_file));
    //mktemp函数根据该模板创建一个唯一的路径名
    lock_fd = Mkstemp(lock_file);
    //创建一个具备路径名的文件并立即unlink掉
    Unlink(lock_file);			/* but lock_fd remains open */

    //初始化lock_it，用于上锁文件
	lock_it.l_type = F_WRLCK;
	lock_it.l_whence = SEEK_SET;
	lock_it.l_start = 0;
	lock_it.l_len = 0;

	//初始化unlock_it，用于解锁文件
	unlock_it.l_type = F_UNLCK;
	unlock_it.l_whence = SEEK_SET;
	unlock_it.l_start = 0;
	unlock_it.l_len = 0;
}
/* end my_lock_init */
```

本函数创建一个具备路径名的文件并立即unlink掉。通过从文件系统目录中删除该路径名，以后即使程序崩溃，这个临时文件也完全消失

然而只有有一个或多个进程打开着这个文件（即引用计数大于0），该文件本身就不会被删除。（这也是从某个目录中删除一个路径名（引用计数为0时文件消失）与关闭一个打开着的文件（引用计数减1）的本质差别）

**my_lock_wait函数**：

```c
// 源码： server/lock_fcntl.c

void
my_lock_wait()
{
    int		rc;
    
    while ( (rc = fcntl(lock_fd, F_SETLKW, &lock_it)) < 0) {
		if (errno == EINTR)
			continue;
    	else
			err_sys("fcntl error for my_lock_wait");
	}
}
```

**my_lock_release函数**:

```c
// 源码： server/lock_fcntl.c

void
my_lock_release()
{
    if (fcntl(lock_fd, F_SETLKW, &unlock_it) < 0)
		err_sys("fcntl error for my_lock_release");
}
```

该版本的**优缺点**：

- 优点：可移植到所有POSIX兼容系统

- 缺点：
  - 围绕accept的上锁增加了服务器的进程控制CPU时间
  - 涉及文件系统操作，比较耗时

**连接在子进程中的分布**：操作系统均匀地把文件锁散布到等待进程中

## 30.8 TCP预先派生子进程服务器程序，accept使用线程上锁保护

使用线程上锁保护的方法的**优点**：

- 1.速度比文件上锁快
- 2.不仅适用于同一进城内各个线程之间的上锁，也适用于不同进程之间的上锁

基于30.7版本，需要改变的是3个**上锁函数**：

- 1.`my_lock_init(char*)`:初始化文件锁
- 2.`my_lock_wait()`：获取文件锁
- 3.`my_lock_release()`：释放文件锁

**my_lock_init函数**：

```c
// 源码： server/lock_pthread.c

/* include my_lock_init */
#include	"unpthread.h"
#include	<sys/mman.h>

static pthread_mutex_t	*mptr;	/* actual mutex will be in shared memory */

void
my_lock_init(char *pathname)
{
	int		fd;
	pthread_mutexattr_t	mattr;

	fd = Open("/dev/zero", O_RDWR, 0);

	mptr = Mmap(0, sizeof(pthread_mutex_t), PROT_READ | PROT_WRITE,
				MAP_SHARED, fd, 0);
	Close(fd);

	//以下三步的作用：
	//调用一些Pthread库函数，告诉函数库：
	//这是一个位于共享内存区中的互斥锁
	//将用于不同进程之间的上锁
	//1.为互斥锁以默认属性初始化一个phtread_mutexattr_t结构
	Pthread_mutexattr_init(&mattr);
	//2.赋予该结构PTHREAD_PROCESS_SHARED属性
	//该属性的默认值为PTHREAD_PROCESS_PRIVATE，即只允许单个进程内使用
	Pthread_mutexattr_setpshared(&mattr, PTHREAD_PROCESS_SHARED);
	//3.以mattr中的属性初始化mptr（共享内存区中的互斥锁）
	Pthread_mutex_init(mptr, &mattr);
}
/* end my_lock_init */
```

在不同进程之间共享内存空间，本例中使用mmap函数和/dev/zero设备

**my_lock_wait函数**：

```c
// 源码： server/lock_pthread.c

void
my_lock_wait()
{
	Pthread_mutex_lock(mptr);
}
```

**my_lock_release函数**:

```c
void
my_lock_release()
{
	Pthread_mutex_unlock(mptr);
}
```

## 30.9 TCP预先派生子进程服务器程序，传递描述符

只让父进程调用accept，然后父进程把所接受的已连接套接字“传递”给某个子进程

**优点**：绕过了为所有子进程的accept调用提供上锁保护的可能需求

**缺点**：需要从父进程到子进程的某种形式的描述符传递。会使代码更复杂，因为父进程必须跟踪子进程的忙闲状态，以便给空闲子进程传递新的套接字

前后版本**对比**：

- **之前**：进程无需关心由哪个子进程接收一个客户连接，操作系统处理这个细节：给某个子进程以首先调用accept的机会，或给与某个子进程以所需要的文件锁或互斥锁
- **现在**：必须为每个字进程维护一个信息结构以方便管理。因此需要一个Child结构

**效率**：父进程通过字节流管道把描述符传递给各个子进程，并且各个子进程通过字节流管道写回单个字节，无论比使用**共享内存区中的互斥锁**，还是使用**文件锁**的上锁和解锁相比，都要**更费时**

**连接在子进程中的分布**：越早派生从而在Child结构数组中排位越靠前的子进程处理的客户数越多

**Child结构**：

```c
// 源码： server/child.h

typedef struct {
  pid_t		child_pid;		/* 子进程ID */
  int		child_pipefd;	/* 父进程连接到该子进程的字节流管道描述符 */
  int		child_status;	/* 子进程状态，0 = ready */
  long		child_count;	/* # 子进程已处理客户的计数 */
} Child;

Child	*cptr;		/* array of Child structures; calloc'ed */
```

**child_make函数：**

![](G:\OneDrive\Github\Interview-Summary\pics\network\unp笔记\Pic_30_22_父子进程各自关闭一端后的字节流管道.png)

![](G:\OneDrive\Github\Interview-Summary\pics\network\unp笔记\Pic_30_23_所有子进程都派生之后的各个字节流管道.png)

```c
// 源码： server/child05.c

/* include child_make */
#include	"unp.h"
#include	"child.h"

pid_t
child_make(int i, int listenfd, int addrlen)
{
	//sockfd[0] 父进程使用
	//sockfd[1] 子进程使用
	int		sockfd[2];
	pid_t	pid;
	void	child_main(int, int, int);

	//创建一个字节流管道，是一对Unix域字节流套接字
	Socketpair(AF_LOCAL, SOCK_STREAM, 0, sockfd);

	//父进程
	if ( (pid = Fork()) > 0) {
		//父进程关闭子进程使用的域套接字
		Close(sockfd[1]);
		cptr[i].child_pid = pid;
		cptr[i].child_pipefd = sockfd[0];
		cptr[i].child_status = 0;
		//父进程返回服务器main函数
		return(pid);		/* parent */
	}

	//子进程
	//将流管道的自身的一端复制到标准错误输出
	//这样每个子进程可以通过读写标准错误输出和父进程通信
	Dup2(sockfd[1], STDERR_FILENO);		/* child's stream pipe to parent */
	//子进程关闭父进程使用的域套接字
	Close(sockfd[0]);
	//由于已经将流管道子进程的一端复制到了标准错误输出
	//因此可以关闭sockfd[1]
	Close(sockfd[1]);
	//关闭监听套接字，由父进程进行监听
	Close(listenfd);
	//子进程不返回，进入child_main函数	/* child does not need this open */
	child_main(i, listenfd, addrlen);	/* never returns */
}
/* end child_make */
```

**服务器main函数**：

```c
// 源码： server/serv05.c

/* include serv05a */
#include	"unp.h"
#include	"child.h"

static int		nchildren;

int
main(int argc, char **argv)
{
	int			listenfd, i, navail, maxfd, nsel, connfd, rc;
	void		sig_int(int);
	pid_t		child_make(int, int, int);
	ssize_t		n;
	fd_set		rset, masterset;
	socklen_t	addrlen, clilen;
	struct sockaddr	*cliaddr;

	if (argc == 3)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 4)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv05 [ <host> ] <port#> <#children>");

	FD_ZERO(&masterset);
	//打开监听套接字对应的位
	FD_SET(listenfd, &masterset);
	maxfd = listenfd;
	cliaddr = Malloc(addrlen);

	nchildren = atoi(argv[argc-1]);
	//navail用于跟踪当前可用的子进程数
	navail = nchildren;
	//分配Child结构数组的内存空间
	cptr = Calloc(nchildren, sizeof(Child));

		/* 4prefork all the children */
	for (i = 0; i < nchildren; i++) {
		child_make(i, listenfd, addrlen);	/* parent returns */
		//打开各个子进程的字节流管道对应的位
		FD_SET(cptr[i].child_pipefd, &masterset);
		maxfd = max(maxfd, cptr[i].child_pipefd);
	}

	Signal(SIGINT, sig_int);

	for ( ; ; ) {
		rset = masterset;
		//如果navail为0，表示无子进程“闲置”
		//从select的描述符集中关掉与监听套接字对应的位
		//防止父进程再无可用子进程的情况下accept连接
		//内核仍然将这些连接入队，直到达到listen的backlog数为止
		if (navail <= 0)
			FD_CLR(listenfd, &rset);	/* turn off if no available children */
		//父进程使用select监听“监听套接字”和各个子进程的字节流管道
		nsel = Select(maxfd + 1, &rset, NULL, NULL, NULL);

			/* 4check for new connections */
		//监听套接字可读，至少有一个连接准备好accept
		if (FD_ISSET(listenfd, &rset)) {
			clilen = addrlen;
			connfd = Accept(listenfd, cliaddr, &clilen);

			//找出第一个可用的子进程
			for (i = 0; i < nchildren; i++)
				if (cptr[i].child_status == 0)
					break;				/* available */

			//如果遍历完cptr数组也没有可用的子进程，说明子进程不够用
			if (i == nchildren)
				err_quit("no available children");
			//改变被选中的子进程的状态
			cptr[i].child_status = 1;	/* mark child as busy */
			//更新选中子进程处理客户的统计值
			cptr[i].child_count++;
			//可用子进程数减少1
			navail--;

			//把就绪的已连接套接字传递给选中子进程
			n = Write_fd(cptr[i].child_pipefd, "", 1, connfd);
			//父进程关闭已经传送给子进程的这个已连接套接字
			Close(connfd);
			if (--nsel == 0)
				continue;	/* all done with select() results */
		}

			/* 4find any newly-available children */
		for (i = 0; i < nchildren; i++) {
			//child_main函数调用子进程处理完一个客户后，
			//通过子进程的字节流管道向父进程写回单个字节
			//使得该字节流的父进程拥有端变为可读
			if (FD_ISSET(cptr[i].child_pipefd, &rset)) {
				if ( (n = Read(cptr[i].child_pipefd, &rc, 1)) == 0)
					err_quit("child %d terminated unexpectedly", i);
				//将处理完客户的子进程状态更改为可用
				cptr[i].child_status = 0;
				//递增可用子进程计数
				navail++;
				//select中就绪套接字都处理完，则可以提前终止循环
				if (--nsel == 0)
					break;	/* all done with select() results */
			}
		}
	}
}
/* end serv05a */
```

**child_main函数**：

```c
// 源码： server/child05.c

/* include child_main */
void
child_main(int i, int listenfd, int addrlen)
{
	char			c;
	int				connfd;
	ssize_t			n;
	void			web_child(int);

	printf("child %ld starting\n", (long) getpid());
	for ( ; ; ) {
		//阻塞于read_fd调用，等待父进程传递过来一个已连接套接字描述符
		if ( (n = Read_fd(STDERR_FILENO, &c, 1, &connfd)) == 0)
			err_quit("read_fd returned 0");
		if (connfd < 0)
			err_quit("no descriptor from read_fd");

		web_child(connfd);				/* process request */
		Close(connfd);
		//子进程处理完一个客户后，
		//通过子进程的字节流管道向父进程写回单个字节
		//告知父进程本子进程已可用（闲置）
		//使得该字节流的父进程拥有端变为可读
		//父进程可以更新该子进程的状态变为“可用”
		Write(STDERR_FILENO, "", 1);	/* tell parent we're ready again */
	}
}
/* end child_main */
```

## 30.10 TCP并发服务器程序，每个客户一个线程

如果服务器主机支持线程，我们可以改用线程以取代子进程

为每个客户创建一个线程，以取代为每个客户派生一个子进程

**优点**：简单的创建线程版本快于所有预先派生子进程的版本

**main函数**：

```c
// 源码： server/serv06.c

#include	"unpthread.h"

int
main(int argc, char **argv)
{
	int				listenfd, connfd;
	void			sig_int(int);
	void			*doit(void *);
	pthread_t		tid;
	socklen_t		clilen, addrlen;
	struct sockaddr	*cliaddr;

	if (argc == 2)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 3)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv06 [ <host> ] <port#>");
	cliaddr = Malloc(addrlen);

	Signal(SIGINT, sig_int);

	for ( ; ; ) {
		clilen = addrlen;
		//主线程阻塞于accept
		connfd = Accept(listenfd, cliaddr, &clilen);

		//当主线程返回一个客户连接时，调用Pthread_create创建一个新线程
		//新线程执行的函数是doit，其参数是“已连接套接字”
		Pthread_create(&tid, NULL, &doit, (void *) connfd);
	}
}
```

**doit函数**：

```c
// 源码： server/serv06.c

void *
doit(void *arg)
{
	void	web_child(int);

	//让自己脱离，使得主线程不必等待它
	Pthread_detach(pthread_self());
	//调用客户处理函数
	web_child((int) arg);
	//处理完毕后，该线程关闭“已连接套接字”
	Close((int) arg);
	return(NULL);
}
```

## 30.11 TCP预先创建线程服务器程序，每个线程各自accept

创建一个线程池，并让每个线程各自调用accept，使用互斥锁以保证任何时刻只有一个线程在调用accept

**连接在线程中的分布**：均匀分布

**效率**：所有版本中最快的

**Thread结构**：

用于维护线程若干信息的Thread结构

```c
// 源码： server/pthread07.h

typedef struct {
  pthread_t		thread_tid;		/* 线程ID */
  long			thread_count;	/* # 处理的客户数统计 */
} Thread;
Thread	*tptr;		/* array of Thread structures; calloc'ed */

int				listenfd, nthreads;
socklen_t		addrlen; 
pthread_mutex_t	mlock; //线程锁
```

**服务器main函数**：

```c
// 源码： server/serv07.c

/* include serv07 */
#include	"unpthread.h"
#include	"pthread07.h"

pthread_mutex_t	mlock = PTHREAD_MUTEX_INITIALIZER;

int
main(int argc, char **argv)
{
	int		i;
	void	sig_int(int), thread_make(int);

	if (argc == 3)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 4)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv07 [ <host> ] <port#> <#threads>");
	nthreads = atoi(argv[argc-1]);
	//为存储线程信息的数组分配空间
	tptr = Calloc(nthreads, sizeof(Thread));

	//创建池中的线程
	for (i = 0; i < nthreads; i++)
		thread_make(i);			/* only main thread returns */

	Signal(SIGINT, sig_int);

	for ( ; ; )
		pause();	/* everything done by threads */
}
/* end serv07 */
```

**thread_make函数**：

```c
// 源码： server/pthread07.c

#include	"unpthread.h"
#include	"pthread07.h"

void
thread_make(int i)
{
	void	*thread_main(void *);
	//创建线程，线程ID存于数组中
    //线程执行函数为thread_main，thread_main的参数为该线程的在数组中的下标
	Pthread_create(&tptr[i].thread_tid, NULL, &thread_main, (void *) i);
	return;		/* main thread returns */
}
```

**thread_main函数**：

```c
// 源码： server/pthread07.c

void *
thread_main(void *arg)
{
	int				connfd;
	void			web_child(int);
	socklen_t		clilen;
	struct sockaddr	*cliaddr;

	cliaddr = Malloc(addrlen);

	printf("thread %d starting\n", (int) arg);
	for ( ; ; ) {
		clilen = addrlen;
		//上锁
    	Pthread_mutex_lock(&mlock);
		connfd = Accept(listenfd, cliaddr, &clilen);
		//解锁
		Pthread_mutex_unlock(&mlock);
		tptr[(int) arg].thread_count++;

		web_child(connfd);		/* process request */
		Close(connfd);
	}
}
```

## 30.12 TCP预先创建线程服务器程序，主线程统一accept

只让主线程调用accept并把每个客户连接传递给池中某个可用线程

**问题**：主线程如何把一个已连接套接字传递给线程池中某个可用线程

- 1.使用描述符传递，但所有线程和描述符都在一个进程内，没必要这样做
- 2.将描述符存在一个数组中

**效率**：慢于“TCP预先创建线程服务器程序，每个线程各自accept”版本，原因在于：本例中同时需要互斥量和条件变量，而“每个线程各自accept”版本只需要互斥量

**pthread结构**：

- 相比“各自accept”版本，多出了一个存储已连接描述符的数组，以及相应的互斥量和条件变量

```c
typedef struct {
  pthread_t		thread_tid;		/* thread ID */
  long			thread_count;	/* # connections handled */
} Thread;
Thread	*tptr;		/* array of Thread structures; calloc'ed */

#define	MAXNCLI	32
//由主线程往其中存入已接受的已连接套接字描述符
//由线程池中的线程从中取出一个以服务相应的客户
//iput是主线程将往该数组中存入的下一个元素的下标
//iget是线程池中某个线程从该数组中取出的下一个元素的下标
int					clifd[MAXNCLI], iget, iput;
//互斥量，用户clifd的互斥访问
pthread_mutex_t		clifd_mutex;
//条件变量，用以决定是否还有需要处理的连接
pthread_cond_t		clifd_cond;
```

**服务器main函数**：

```c
/* include serv08 */
#include	"unpthread.h"
#include	"pthread08.h"

static int			nthreads;
pthread_mutex_t		clifd_mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t		clifd_cond = PTHREAD_COND_INITIALIZER;

int
main(int argc, char **argv)
{
	int			i, listenfd, connfd;
	void		sig_int(int), thread_make(int);
	socklen_t	addrlen, clilen;
	struct sockaddr	*cliaddr;

	if (argc == 3)
		listenfd = Tcp_listen(NULL, argv[1], &addrlen);
	else if (argc == 4)
		listenfd = Tcp_listen(argv[1], argv[2], &addrlen);
	else
		err_quit("usage: serv08 [ <host> ] <port#> <#threads>");
	cliaddr = Malloc(addrlen);

	nthreads = atoi(argv[argc-1]);
	tptr = Calloc(nthreads, sizeof(Thread));
	iget = iput = 0;

		/* 4create all the threads */
	for (i = 0; i < nthreads; i++)
		thread_make(i);		/* only main thread returns */

	Signal(SIGINT, sig_int);

	for ( ; ; ) {
		clilen = addrlen;
		connfd = Accept(listenfd, cliaddr, &clilen);

		//获取对clifd的互斥访问
		Pthread_mutex_lock(&clifd_mutex);
		//将主线程accept的已连接套接字描述符存入clifd数组中
		clifd[iput] = connfd;
		//如果iput达到最大值，循环使用
		if (++iput == MAXNCLI)
			iput = 0;
		//检查iput下标是否赶上iget下标
		//如果赶上，说明数组不够大
		if (iput == iget)
			err_quit("iput = iget = %d", iput);
		//释放信号量，如果有子进程睡眠于条件变量，则唤醒线程
		Pthread_cond_signal(&clifd_cond);
		//释放对clifd的互斥访问
		Pthread_mutex_unlock(&clifd_mutex);
	}
}
/* end serv08 */
```

**thread_make函数**：

```c
// 源码： server/pthread08.c

#include	"unpthread.h"
#include	"pthread08.h"

void
thread_make(int i)
{
	void	*thread_main(void *);
	//创建线程，将线程ID存于tptr数组中
    //线程的执行函数为thread_main，该函数的参数为线程在tptr数组中的下标
	Pthread_create(&tptr[i].thread_tid, NULL, &thread_main, (void *) i);
	return;		/* main thread returns */
}
```

**thread_main函数**：

```c
// 源码： server/pthread08.c

void *
thread_main(void *arg)
{
	int		connfd;
	void	web_child(int);

	printf("thread %d starting\n", (int) arg);
	for ( ; ; ) {
		//获取clifd的互斥访问
    	Pthread_mutex_lock(&clifd_mutex);
    	//如果已经没有需要处理的已连接套接字
		while (iget == iput)
			//将该进程睡眠于条件变量
			//如果主线程接受一个连接，则会释放条件变量
			//从而唤醒睡眠在该条件变量上的线程（一次只唤醒一个）
			Pthread_cond_wait(&clifd_cond, &clifd_mutex);
		//取出一个已连接套接字
		connfd = clifd[iget];	/* connected socket to service */
		if (++iget == MAXNCLI)
			iget = 0;
		Pthread_mutex_unlock(&clifd_mutex);
		tptr[(int) arg].thread_count++;
		//线程处理已连接套接字
		web_child(connfd);		/* process request */
		Close(connfd);
	}
}
```

## 30.13 小结

- 1.当系统负载较轻时，每来一个客户请求，现场派生一个子进程为之服务的传统并发服务器程序模式就足够了
- 2.预先创建一个子进程池或一个线程池的设计范式能把进程控制CPU时间降低10倍或以上。但本书中例子没实现的是：监视闲置子进程个数，随着服务客户数的动态变化而增加或减少这个数目
- 3.某些内核实现允许多个子进程或线程阻塞于同一个accept调用，有一些不能。因此需要包绕accept调用安置某些类型的锁加以保护，如文件锁或Pthread互斥锁
- 4.让所有子进程或线程自动调用accept比让父进程或主线程独自调用accept并把描述符传递给子进程或线程来得简单而快速
- 5.由于潜在select冲突的原因，让所有子进程或线程阻塞于同一个accept调用比让它们阻塞在同一个select调用更可取
- 6.使用线程通常比使用进程更快，但需要考虑两个问题：
  - 操作系统是否支持多线程
  - 服务每个客户是否需要激活其他程序。如果accept客户连接的服务器调用fork和exec（譬如inetd超级守护进程），那么fork一个单线程的进程可能快于fork一个多线程的进程










