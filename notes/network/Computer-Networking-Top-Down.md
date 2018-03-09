<!-- GFM-TOC -->
* [第 2 层 应用层](#第-2-层-应用层)
    * [2.1 应用层协议原理](#21-应用层协议原理)
    * [2.2 Web 和 HTTP](#22-web-和-http)
    * [2.3 文件传输协议：FTP](#23-文件传输协议ftp)
    * [2.4 因特网中的电子邮件](#24-因特网中的电子邮件)
        * [2.4.1 SMTP](#241-smtp)
        * [2.4.4 邮件访问协议](#244-邮件访问协议)
    * [2.5 DNS:因特网的目录服务](#25-dns:因特网的目录服务)
* [第 3 层 运输层](#第-3-层-运输层)
    * [3.4 可靠数据传输原理](#34-可靠数据传输原理)
    * [3.5 面向连接的运输：TCP](#35-面向连接的运输tcp)
        * [3.5.5 流量控制](#355-流量控制)
        * [3.5.6 TCP 连接管理](#356-tcp-连接管理)
    * [3.6 拥塞控制原理](#36-拥塞控制原理)
    * [3.7 TCP 拥塞控制](#37-tcp-拥塞控制)
* [第5章 链路层概述](#第5章-链路层概述)
* [5.7 回顾 Web 页面请求的历程](#57-回顾-web-页面请求的历程)
        * [DHCP：获得 IP 本机地址](#dhcp获得-ip-本机地址)
        * [DNS、ARP：获取 www.google.com 的 IP 地址](#dnsarp获取-wwwgooglecom-的-ip-地址)
        * [TCP、HTTP：Web 客户-服务器交互](#tcphttpweb-客户-服务器交互)
<!-- GFM-TOC -->


# 第 2 层 应用层

## 2.1 应用层协议原理

客户-服务器体系结构：Web、FTP、Telnet 和电子邮件。

P2P：文件共享（BitTorrent）、对等协助加速器（迅雷）、因特网电话（Skype)、IPTV(迅雷看看、PPStream)。

## 2.2 Web 和 HTTP

因为 HTTP 服务器并不保存关于客户的任何信息，因此 HTTP 是一个无状态协议。

HTTP 可以使用 **非持续连接** ，也可以使用**连续连接**。默认为后者。

## 2.3 文件传输协议：FTP

FTP 是有状态的

包括控制连接（由客户端发起，持续整个会话）和数据连接（有服务器发起，每传输完一次都会关闭，即是非持续的）

## 2.4 因特网中的电子邮件

### 2.4.1 SMTP

SMTP 用的是持续连接。

### 2.4.4 邮件访问协议

推协议：SMTP

拉协议：第三版邮局协议 POP3（Post Office Protocol--Version 3）、因特网邮局访问协议 IMAP（Internet Mail Access Protocol）、HTTP

POP3：不维护会话状态信息

IMAP:维护会话的额用户状态信息

## 2.5 DNS:因特网的目录服务

**DNS 是：** 

-   1.一个由分层的 DNS 服务器实现的分布式数据库
-   2.一个使得主机能够查询分布式数据库的应用层协议（运行在 UDP 之上）

**DNS 提供的服务：** 

-   1.主机名到 IP 地址的转换
-   2.主机别名：获得主机别名对应的规范主机名以及主机的 IP 地址
-   3.邮件服务器别名：对提供的邮件服务器别名进行解析，以获得该主机的规范主机名及其 IP 地址，允许邮件服务器和 Web 服务器使用相同（别名化的）的主机名
-   4.负载分配

**3 种类型的 DNS 服务器：** 

-   根 DNS 服务器
-   顶级域名（Top-Level Domain,TLD）DNS 服务器
-   权威 DNS 服务器
-   本地 DNS 服务器（默认名字服务器，严格说不属于 DNS 服务器的层次结构）

DNS 利用递归查询（主机与本地 DNS 服务器之间）和迭代查询

DNS 资源记录：(Name, Value, Type, TTL)

**Type:** 

-   A:则 Name 是主机名，Value 是该主机名对应的 IP 地址
    -   (relay1.bar.foo.com, 145.37.93.126, A)
-   NS：则 Name 是一个域，Value 是一个如何获得该域中主机 IP 地址的权威 DNS 服务器的主机名
    -   (foo.com, dns.foo.com, NS)
-   CNAME:则 Name 为主机别名，Value 为对应的规范主机名
    -   (foo.com, relay1.bar.foo.com, CNAME)
-   MX:则 Name 邮件服务器的别名，Value 为对应的规范主机名
    -   (foo.com, mail.bar.foo.com, MX)

一个公司的邮件服务器和其他服务器（如 Web 服务器）可以使用相同的别名。为了获得邮件服务器的规范主机名，DNS 客户请求一条 MX 记录；为了获得其他服务器的规范主机名，DNS 客户请求一条 CNAME 记录。

**向 DNS 数据库插入一条记录** 

-   1.向注册登记机构提供你的主机域名、权威服务器的域名、权威服务器的 IP，注册登记机构将这些资源记录插入所有 TLD com 服务器。假设你的域名为 networkutopia.com，则有：
    -   (networkutopia.com, dns1.networkutopia.com, NS)
    -   (dns1.networkutopia.com, 212.212.212.1, A)
-   2.在你的权威服务器中保存：
    -   (networkutopia.com, 211,211,211,211, A)
    -   (networkutopia.com, mail.networkutopia.com, MX)
    -   (mail.networkutopia.com, 211,211,211,212, A)

# 第 3 层 运输层

## 3.4 可靠数据传输原理

**回退 N 步-GBN** 

-   发送端：
    -   只有一个定时器
    -   仅维护已发送但未被确认的字节的最小序号（SendBase）和下一个要发送但未被确认的字节的最小序号（NextSeqNum）
    -   无累积确认
-   接收端：
    -   仅维护下一个按序接收的分组的序号（expectedseqnum）
    -   不缓存乱序分组，乱序分组到达时丢弃并发送 ACK(expectedseqnum)
-   重传时：
    -   定期器超时，从 Sendbase 到 NextSeqNum-1 的分组全部重传

**选择重传** 

-   发送端：
    -   为每个分组维护一个定时器
    -   维护 SendBase 和 NextSeqNum
    -   无累积确认
-   接收端
    -   缓存乱序分组，维护一个接收窗口，rcv_base 和窗口长度 N
    -   乱序分组到达时，若分组序号在接收窗口内，返回该分组的 ACK；若在接收窗口左侧，也会产生该分组对应的 ACK
-   重传时：
    -   哪个分组的定时器超时，就重传哪个分组

**TCP 可靠数据传输** 

-   发送端：
    -   只有一个定时器
    -   仅维护已发送但未被确认的字节的最小序号（SendBase）和下一个要发送但未被确认的字节的最小序号（NextSeqNum）
    -   有累积确认
-   接收端：
    -   缓存乱序分组，维护一个接收窗口，rcv_base 和窗口长度 N
    -   乱序分组到达时，若分组序号在接收窗口内，返回该分组的 ACK
-   重传时：
    -   发生条件：超时或 3 个冗余 ACK（即总 4 个 ACK）
    -   因为有累积确认，若 1，2，……，N 中只有 n（n<N）未确认，发生超时时，只重传 n
    -   超时加倍，但一旦受到一个 SampleRTT，就将超时设为`TimeoutInterval=EstimatedRTT+4*DevRTT`

TCP 可靠数据传输是 GBN 和选择重传的结合

## 3.5 面向连接的运输：TCP

### 3.5.5 流量控制

**流量控制与拥塞控制的区别** 

流量控制控制发送方的发送速率与接收方应用程序读取速度不匹配的情况。

拥塞控制：TCP 发送方因 IP 网络的拥塞而被遏制。

### 3.5.6 TCP 连接管理

#### 三次握手

**1.客户端---> 服务器** 

-   向服务器发送 SYN 报文段（同步报文段），该报文段的特点：
    -   不包含应用层数据
    -   SYN 设为 1
    -   随机选择一个初始序号（`client_isn`），放入序号字段中

**2.服务器---> 客户端** 

-   提取 TCP SYN 报文
-   为该 TCP 连接分配 TCP 缓存和变量（在完成三次握手的第三步之前分配这些缓存和变量，会使得 TCP 易于受到 SYN 洪泛的拒绝服务器攻击）。
-   向客户 TCP 发送允许连接的 SYNACK 报文段，该报文段的特点：
    -   不包含应用层数据
    -   SYN 设为 1
    -   确认号字段设为`client_isn+1`
    -   选择自己的初始序号（`server_isn`），放入序号字段中

**3.客户端---> 服务器** 

-   收到 SYNACK 报文段
-   客户端给该连接分配缓存和变量
-   向服务器发送另外一个报文段，对服务器的允许连接的报文段进行确认，该报文段的特点：
    -   可以携带客户端到服务器的数据
    -   SYN 为 0
    -   将确认号设为`server_isn+1`
    -   将序列号设为`client_isn+1`

完成以上 3 步，客户端就和服务器建立好连接，之后的报文段的 SYN 都为 0.

#### 四次挥手

**1.客户端---> 服务器** 

-   客户应用程序进程发出一个关闭连接命令
-   发送一个 FIN 设为 1 的特殊报文段

**2.服务器---> 客户端** 

-   回送一个 ACK 确认报文段

**3.服务器---> 客户端** 

-   发送一个 FIN 设为 1 的报文段

**4.客户端---> 服务器** 

-   回送一个 ACK 确认报文段

完成以上步骤，两台主机在定时等待后释放用于该连接的所有资源。

-   客户端三次握手和四次挥手 TCP 状态变迁图
-   图中，在`TIME_WAIT`状态时，客户端回送服务器的 ACK 丢失，客户端会重传最后的确认报文
-   `TIME_WAIT`状态中锁消耗的时间与具体实现有关，典型值为 30 秒、1 分钟或 2 分钟
-   经过等待后，连接正式关闭，客户端所有资源（包括端口号）将被释放

![](https://raw.githubusercontent.com/guanjunjian/Interview-Summary/master/pics/network/Computer-Networking-Top-Down/img_3_41.png)

-   服务器三次握手和四次挥手 TCP 状态变迁图

![](https://raw.githubusercontent.com/guanjunjian/Interview-Summary/master/pics/network/Computer-Networking-Top-Down/img_3_42.png)

#### 一台主机接收到端口或源地址与本机套接字不匹配的情况

**TCP** 

-   发送 RST 为 1 的报文段，告诉源“我没有那个报文段的套接字，请不要再发送该报文段了”

**UDP** 

-   发送特殊的 ICMP 数据报

## 3.6 拥塞控制原理

**拥塞网络的代价：** 

-   1.当分组的到达速率接近链路容量时，分组经历巨大的排队延迟
-   2.发送方必须执行重传以补偿因缓存溢出而丢弃（丢失）的分组
-   3.发送方在遇到大时延时所进行的不必要重传会引起路由器利用其链路带宽来转发不必要的分组副本
-   4.当一个分组沿一条路径被丢弃时，每个上游路由器用于转发该分组到丢弃该分组而适用的传输容量最终被浪费掉了

**拥塞控制方法** 

根据网络层是否为运输层拥塞控制提供了显示帮助来区分：

1.端到端拥塞控制，网络层没有为运输层拥塞提供显示支持。通过对网络行为的观察判断拥塞。

-   如：TCP
    -   拥塞迹象：TCP 报文段丢失（超时或 3 次冗余确认，即 4 次 ACK）
    -   拥塞程度增加的指示：添加的往返时延

2.网络辅助的拥塞控制，网络层向发送方提供关于网络中拥塞的显示反馈信息。如 ATM ABR。

-   拥塞信息从网络反馈到发送方的两种方式：
    -   路由器发送给发送方一个阻塞分组
    -   路由器标记或更新从发送方向接收方的分组中的某个字段来指示拥塞的产生，然后接收方向发送方通知该网络拥塞指示。

## 3.7 TCP 拥塞控制

在发送方

1.限制流量的方法：拥塞窗口 cwnd

```
LastByteSent - LastByteAcked <= min{ cwnd, rwnd }
```

2.感知拥塞：

-   拥塞指示：丢包事件，即超时或收到 3 个冗余 ACK
-   正常指示：收到未确认报文段的 ACK

3.如何确定发送速率

-   报文段丢失（超时或收到 3 个冗余 ACK），拥塞，降低 TCP 发送速率
-   收到未确认报文段的 ACK，增加发送方的速率
-   宽带探测，增加其速率以响应到达的 ACK，除非出现丢包事件，才减小传输速率

4.TCP 拥塞控制算法：慢启动、拥塞避免、快速恢复

-   强制部分：慢启动、拥塞避免
    -   它俩的差异在于收到 ACK 做出反应时增加 cwnd 长度的方式，慢启动更快地增长 cwnd
-   推荐部分：快速恢复，非必需的

---

![](https://raw.githubusercontent.com/guanjunjian/Interview-Summary/master/pics/network/Computer-Networking-Top-Down/img_3_52.png)

#### 慢启动

**进入慢启动的情况：** 

1.初始情况

-   { cwnd = 1, ssthresh = 64KB, dupACKcount = 0 }

2.慢启动---> 慢启动；拥塞避免---> 慢启动；快速恢复---> 慢启动

-   条件：超时
-   { ssthresh = cwnd/2, cwnd = 1MSS, dupACKcount = 0, 重传丢失报文段 }

**离开慢启动的情况：** 

1.慢启动---> 拥塞避免

-   条件：cwnd >= ssthresh

2.慢启动---> 快速恢复

-   条件：dupACKcount == 3，3 个冗余 ACK
-   { ssthresh = cwnd / 2, cwnd = ssthread + 3 * MSS, 重传丢失报文段 }

**慢启动的增长模式：** 

-   条件：新的 ACK 到达
-   { cwnd = cwnd + MSS, dupACKcount = 0, 如果条件允许，传送新报文段 }

#### 拥塞避免

加性增，乘性减。

**进入拥塞避免的情况：** 

1.慢启动---> 拥塞避免

-   条件：cwnd >= ssthresh

2.快速恢复---> 拥塞避免

-   条件：新 ACK 到达
-   { cwnd = ssthresh, dupACKcount = 0 }

**离开拥塞避免的情况** 

1.拥塞避免---> 慢启动

-   条件：超时
-   { ssthresh = cwnd / 2, cwnd = 1MSS, dupACKcount = 0, 重传丢失报文段 }

2.拥塞避免---> 快速恢复

-   条件：dupACKcount == 3，3 个冗余 ACK
-   { ssthresh = cwnd / 2, cwnd = ssthread + 3 * MSS, 重传丢失报文段 }

**拥塞避免的增长模式：** 

-   条件：新的 ACK 到达
-   { cwnd = cwnd + MSS * ( MSS / cwnd ), dupACKcount = 0, 如果条件允许，传送新报文段 }

#### 快速恢复

**进入快速恢复的情况：** 

1.慢启动---> 快速恢复；拥塞避免---> 快速恢复

-   条件：dupACKcount == 3，3 个冗余 ACK
-   { ssthresh = cwnd / 2, cwnd = ssthread + 3 * MSS, 重传丢失报文段 }

**离开快速恢复的情况** 

1.快速恢复---> 拥塞避免

-   条件：新 ACK 到达
-   { cwnd = ssthresh, dupACKcount = 0 }

2.快速恢复---> 慢启动

-   条件：超时
-   { ssthresh = cwnd / 2, cwnd = 1MSS, dupACKcount = 0, 重传丢失报文段 }

**快速恢复的增长模式** 

-   条件：冗余 ACK
-   { cwnd = cwnd + MSS, 如果条件允许，传送新报文段 }

# 第5章 链路层概述

# 5.7 回顾 Web 页面请求的历程

![](https://raw.githubusercontent.com/guanjunjian/Interview-Summary/master/pics/network/Computer-Networking-Top-Down/img_5_32.png)

### DHCP：获得 IP 本机地址

-   1.Bob 主机的操作系统生成一个 DHCP 请求报文。
-   2.包含 DHCP 请求报文的 IP 数据报被放置在以太网帧中。
-   3.以太网帧在交换机的所有出端口广播该以太网帧，保留连接到路由器的端口。
-   4.路由器（DHCP 服务器）收到该以太网帧。
-   5.运行在路由器中的 DHCP 服务器能够以 CIDR 块分配 IP 地址。HDCP 服务器生成包含这个 IP 地址、DNS 服务器 IP 地址、默认网关路由器的 IP 地址、子网块（子网掩码）的一个 DHCP ACK 报文。该报文最后放入以太网帧中。
-   6.以太网帧由路由器发给交换机，交换机从通向目的主机（最初发送 DHCP 请求的主机）的输出端口转发。
-   7.Bob 主机收到 DHCP ACK，记录下它的 IP 地址、DNS 服务器 IP 地址，并在其 IP 转发表中安装默认路由网关的地址。

### DNS、ARP：获取 www.google.com 的 IP 地址

分为两部分：获取网关路由器的 MAC 地址、发送 DNS 查询报文。

#### 第一部分.ARP：获取网关路由器的 MAC 地址

-   8.Bob 生成一个 DNS 查询报文，该报文的目的 IP 地址为 DNS 服务器的 IP 地址。
-   9.将 DNS 请求报文放入一个以太网帧中，需要将该以太网帧发往默认网关路由器，但不知道网关路由器的 MAC 地址。为了获得网关路由器的 MAC 地址，需要使用 ARP 协议。
-   10.Bob 生成一个具有目的 IP 地址为默认网关 IP 地址的 ARP 查询报文，放入广播太网帧中，向交换机发送该以太网帧。交换机将该帧交付给所有连接的设备（广播），包括网关路由器。
-   11.网关路由器收到该 ARP 查询报文的帧，准备一个 ARP 回答，该回答包含了网关路由器的 MAC。将 ARP 回答放入以太网帧中，该帧的目的 MAC 地址为 Bob 主机的 MAC 地址，向交换器发送该帧。交换器再将该帧交付给 Bob 主机。
-   12.Bob 主机接收到包含 ARP 回答的帧，抽取网关路由器的 MAC 地址。
-   13.现在，可以发送 DNS 查询报文了，该报文的 IP 目的地址为 DNS 服务器的 IP 地址，而 MAC 目的地址为网关路由器的 MAC 地址。Bob 主机向交换机发送包含 DNS 查询报文的以太网帧。交换机再将该帧交付给网关路由器。

#### 第二部分.DNS：获取 www.google.com 的 IP 地址

-   14.网关路由器接收该帧并抽取包含 DNS 查询的 IP 数据报，根据其转发表决定该数据报应发送到图中 Comcast 网络中最左边的路由器。
-   15.在 Comcast 网络中最左边的路由器接收到该帧，抽取 IP 数据报，根据其转发表，朝着 DNS 服务器转发数据报。而转发表已根据 Comcast 的域内协议（RIP、OSPF 或 IS-IS）以及因特网的域间协议 BGP 所填写。
-   16.包含 DNS 查询的 IP 数据报到达 DNS 服务器。DNS 服务器在其数据库中查找`www.google.com`。找到 DNS 源记录，形参一个 DNS 回答报文，放入 UDP 报文段，寻址到 Bob 的主机。反向转发到 Bob 的网关路由器。
-   17.Bob 主机收到 DNS 回答报文，抽取出 www.google.com 的 IP 地址。

### TCP、HTTP：Web 客户-服务器交互

-   18.根据 google IP 生成 TCP 套接字，用于向 google 发送 HTTP GET 报文。需要与 google 进行 TCP 三次握手：Bob 生成 TCP SYN 报文段，目的 IP 地址为 google IP，目的 MAC 地址为网关路由器，并向交换机发送该帧。
-   19.在学生网络、Comcast 网络、谷歌网络中的路由器朝着 www.google.com 转发包含 TCP SYN 的数据报。
-   20.包含 TCP SYN 的数据报到达`www.google.com`。抽取 TCP SYN，分解到与 80 端口相联系的欢迎套接字，并生成一个连接套接字。产生一个 TCP SYNACK 报文段，向 Bob 发送。
-   21.包含 TCP SYNACK 报文段的数据报通过谷歌、Comcast 和学校网络，到达 Bob。
-   22.借助套接字，Bob 生成要获取的 URL 的 HTTP GET 报文，写入套接字，交付到 google。
-   23.google 的 HTTP 服务器从 TCP 套接字读取 HTTP。 GET 报文，生成 HTTP 响应报文，将请求的 Web 内容写入 HTTP 响应体重，并将报文发送进 TCP 套接字中。
-   24.包含 HTTP 回答报文的数据报通过谷歌、Comcast、学校网络转发，到达 Bob。Bob 的 Web 浏览器从套接字读取 HTTP 响应，从 HTTP 响应体中抽取 Web 网页的 html，并最终显示了 Web 网页。
