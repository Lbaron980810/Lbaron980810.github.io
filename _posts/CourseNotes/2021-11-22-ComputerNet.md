---
title: 计算机网络课程笔记
categories: CourseNotes
---
# Chapter1: 概论

计算机网络——什么是Internet？

- 节点：

  - 主机节点（方形表示）：源节点，目标节点

  - 数据交换节点（圆形表示）

- 边：通信链路

  - 接入网链路：主机连接到互联网的链路（连接方形和圆形的）
  -  干链路：路由器间的链路 
  - 传输速率：带宽（bps）

- 分组交换设备：转发分组

  - 交换机：工作在链路层的网络设备
  - 路由器：工作在网络层的网络设备

- 协议：对等层的实体，在通信的过程中，应当遵守的规则集合

  - TCP, IP, HTTP, FTP, PPP
  - 定义了报文格式、次序、动作

- Internet标准

  - RFC: Request for comments
  - IETF: Internet Engineering Task Force

互联网：以TCP/IP协议为主的一小簇协议支撑的网络

网络结构：

- 网络边缘edge：主机、应用程序
  - 端系统（主机）
  - 客户/服务器模式
    - 客户端向服务器请求、接受服务
  - 对等（peer-peer P2P）模式
    - 很少、甚至没有专门的服务器
  - 采用网络设施的面向连接服务（TCP ）
  - 采用网络设施的无连接服务（UDP ）
- 网络核心core：互连着的路由器，网络的网络  
- 接入网、物理媒体access：有线或者无线通信链路





## 1.3 网络核心

- 网络核心core：互连着的路由器，网络的网络 
- 基本问题：数据怎样通过网络进行传输？
  - 电路（circuit）交换（一般用于电话网 ）：为每个呼叫预留一条专有电路
  - 分组（packet）交换（互联网和几乎所有网络都采用这个）：
    - 将要传送的数据分成一个个单位，分组
    - 将分组从一个路由器传到相邻路由器，一段段最终从源端传到目标端
    - 每段：采用链路的最大传输能力（带宽）

电路交换分段方式：FDM 频分、TDM 时分、WDM 波分、 CDM 

分组交换：不分段，一次使用全部的带宽，每个节点先存储再转发

![Screenshot from 2021-11-22 22-40-27](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-22%2022-40-27.png)

网络核心的关键功能：

- 路由：决定分组采用的源到目标的路径
- 转发：将分组从路由器的输入链路转移到输出链路

![Screenshot from 2021-11-22 22-47-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-22%2022-47-03.png)



分组交换：分组的存储转发一段一段从源端传到目标端，按照有无网络层的连接，分成：

- 数据报网络
  - 分组的目标地址决定下一跳
  - 在不同的阶段、路由可以改变
  - 类似问路
  - Internet
- 虚电路网络
  - 每个分组都带标签（虚电路标识VCID）、标签决定下一跳
  - 在呼叫建立时决定路径，在整个呼叫中路径保持不变
  - 路由器维持每个呼叫的状态信息
  - X.25和ATM



## 1.4 接入网和物理媒体



## 1.5 Internet结构和ISP

互联网络结构：网络的网络

- 端系统通过接入ISPs(Internet Service Providers)连接到互联网
- 接入ISPs相应的必须是互联的 
  - 中心：第一层ISP
  - 第二层ISP：更小的、区域性的ISP，与第一层或第二层ISP互联
  - 第三层ISP与其他本地ISP：接入网（与端系统最近）

![Screenshot from 2021-11-23 19-34-26](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-23%2019-34-26.png)

ISP之间的连接：

![Screenshot from 2021-11-23 19-53-09](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-23%2019-53-09.png)





## 1.6 分组延时、丢失和吞吐量

分组丢失：分组到达时，没有可用的缓冲区，则分组被丢掉

延时：节点处理延时、分组排队延时、传输延时、传播延时

吞吐量：瓶颈链路——整条链路上吞吐量最小的链路

 

## 1.7 协议层次及服务模型

层次化方式实现复杂网络功能：

- 将网络复杂的功能分成功能明确的层次，每一层实现了其中一个或一组功能，功能中有其上层可以使用的功能：服务
- 本层协议实体相互交互执行本层的协议动作，目的是实现本层功能，通过接口为上层提供更好的服务。
- 在实现本层协议的时候，直接利用了下层所提供的服务
- 本层的服务：借助下层服务实现的本层协议实体之间交互带来的新功能（上层可以利用的）+更下层所提供的服务



服务和服务访问点

- 服务（service）：低层实体向上层实体提供他们之间的通信的能力
  - 服务用户
  - 服务提供者
- 原语（Primitive）：上层使用下层服务的形式，高层使用低层提供的服务，以及低层向高层提供服务都是通过服务访问原语来进行交互的——形式
- 服务访问点SAP（Services Access Point）：上层使用下层提供的服务通过层间的接口——地点

服务的类型

- 面向连接的服务和无连接的服务——方式




服务和协议

- 服务与协议的区别：
  - 服务（service）：低层实体向上层实体提供他们之间的通信的能力，是通过原语（primitive）来操作的，垂直
  - 协议（porotocol）：对等层实体（peer entity）之间在相互通信的过程中，需要遵循的规则的集合，水平
- 服务与协议的联系
  - 本层协议的实现要靠下层提供的服务来实现
  - 本层实体通过协议为上层提供更高级的服务



数据单元（DU）

![Screenshot from 2021-11-25 20-26-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-25%2020-26-34.png)



Internet 协议栈（五层模型）

- 应用层：网络应用——数据单元：报文（message）
  - 为人类用户或者其他应用进程提供网络应用服务
  - FTP, SMTP, HTTP, DNS
- 传输层：主机之间的数据传输——数据单元：报文段（segment）：TCP段，UDP数据报
  - 在网络层提供的端到端通信基础上，细分为进程到进程，将不可靠的通信变成可靠的通信
  - TCP, UDP
- 网络层：为数据报从源到目的选择路由——数据单元：分组packet（如果无连接方式：数据报datagram）
  - 主机之间的通信，端到端通信，不可靠
  - IP，路由协议
- 链路层：相邻网络节点间的数据传输——数据单元：帧（frame）
  - 2个相邻点的通信，点到点通信，可靠或不可靠
  - 点对点协议PPP，802.11（wifi），Ethernet
- 物理层：在线路上传送bit——数据单元：位（bit）



![Screenshot from 2021-11-25 20-41-51](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-25%2020-41-51.png)

## 1.9 小结

![Screenshot from 2021-11-27 10-15-38](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-27%2010-15-38.png)

![Screenshot from 2021-11-27 10-15-45](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-27%2010-15-45.png)







# Chapter2: 应用层

## 2.1 应用层协议原理

  网络应用的体系结构

- 客户-服务器模式（C/S: client / server）
  - 服务器
    - 一直运行
    - 固定IP和约定好的端口号
    - 扩展性：数据中心进行扩展，扩展性差
  - 客户端
    - 主动与服务器通信
    - 与互联网间歇性链接
    - 可能是动态IP地址
    - 不直接与其他客户端通信
- 对等模式（P2P: Peer To Peer）
  - 没有一直运行的服务器
  - 任意端系统之间可以进行通信
  - 每个节点既是客户端又是服务器
  - 参与的主机间歇性连接且可以改变IP地址
    - 难以管理，只有上线的时候才能提供服务
- 混合体：两者结合
  - 例子：Napster
    - 文件搜索：集中
      - 主机在中心服务器上注册其资源
      - 主机向中心服务器查询资源位置
    - 文件传输：P2P
  - 即时通信
    - 在线监测：集中
      - 用户上线时，向中心服务器注册其IP地址
      - 用户与中心服务器联系，以找到其在线好友的位置
    - 两个用户之间聊天：P2P

进程通信：

​	进程：在主机上运行的应用程序

​		客户端进程：发起通信的进程

​		服务器进程：等待连接的进程

- 在同一个主机内，使用进程间通信机制通信（操作系统定义）
- 在不同主机内，通过交换报文（message）来通信
  - 使用OS提供的通信服务
  - 按照应用协议交换报文，借助传输层提供的服务

![Screenshot from 2021-11-30 21-45-38](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2021-45-38.png)

![Screenshot from 2021-11-30 21-49-38](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2021-49-38.png)

![Screenshot from 2021-11-30 21-52-24](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2021-52-24.png)

![Screenshot from 2021-11-30 21-58-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2021-58-00.png)

TCP Socket: 代表源IP，源端口，目标IP，目标端口的一个本地标识，从而简化进程发送的内容，便于管理，使得穿过层间（应用层与传输层）的信息量最少。是应用层与传输层之间的一个约定。

![Screenshot from 2021-11-30 22-03-20](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-03-20.png)

![Screenshot from 2021-11-30 22-05-07](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-05-07.png)

![Screenshot from 2021-11-30 22-08-21](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-08-21.png)

TCP Socket包含的不只是本机的IP和端口，还标识通信对方的IP和端口信息

![Screenshot from 2021-11-30 22-10-22](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-10-22.png)

UDP Socket只包括本机的IP和端口，因此发送时还是需要对方的IP和端口

![Screenshot from 2021-11-30 22-14-05](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-14-05.png)

![Screenshot from 2021-11-30 22-15-02](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-15-02.png)

![Screenshot from 2021-11-30 22-15-45](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-15-45.png)

![Screenshot from 2021-11-30 22-17-33](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-17-33.png)

![Screenshot from 2021-11-30 22-17-47](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-17-47.png)

![Screenshot from 2021-11-30 22-19-39](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-19-39.png)

![Screenshot from 2021-11-30 22-22-33](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-22-33.png)

![Screenshot from 2021-11-30 22-25-20](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-25-20.png)

![Screenshot from 2021-11-30 22-26-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-11-30%2022-26-03.png)

https -> http over ssl，跑在SSL上的http，更安全



## 2.2 Web and HTTP

术语：

- Web页：由一些对象组成
- Web页含有一个基本的HTML文件，该基本HTML文件有包含若干对象的引用（链接）
- 通过URL对每个对象进行引用
  - 访问协议，用户名，口令字，端口等
- URL格式：

![Screenshot from 2021-12-01 19-44-15](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-01%2019-44-15.png)



- HTTP: 超文本传输协议
  - Web的应用层协议
  - 客户/服务器模式
    - 客户端是浏览器
  - 使用TCP
    - 客户发起一个与服务器的TCP连接
    - 服务器接收连接
    - 在浏览器与Web服务器交换HTTP报文
    - TCP关闭连接
  - HTTP是无状态的：服务器不需要维护客户端的任何信息
  - 持久HTTP与非持久HTTP
    - 非持久HTTP：
      - 最多只有一个对象在TCP连接上发送
      - 下载多个对象需要多个TCP连接
      - HTTP/1.0使用
    - 持久HTTP：
      - 多个对象可以在一个TCP连接上传输
      - HTTP/1.1使用



![Screenshot from 2021-12-01 20-05-29](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-01%2020-05-29.png)

- 持久HTTP
  - 非持久HTTP的缺点：
    - 每个对象要两个RTT
    - 操作系统必须为每个TCP连接分配资源
    - 浏览器通常打开并行TCP连接，以获取引用对象
  - 持久HTTP：
    - 服务器在发送响应后，仍保持TCP连接
    - 在相同的CS之间的后续请求和响应报文通过相同的连接进行传送
    - 客户端在遇到一个引用对象的时候，就可以尽快发送该对象的请求
    - 非流水方式的持久HTTP
      - 客户端只能在收到前一个响应后才发出新的请求
      - 每个引用对象花费一个RTT
    - 流水方式的持久HTTP
      - HTTP 1.1的默认模式
      - 客户端遇到一个引用对象就立即产生一个请求
      - 所有引用对象只花费一个RTT是可能的

HTTP请求报文

- 两种类型的HTTP报文：请求、相应

![Screenshot from 2021-12-01 20-12-34](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-01%2020-12-34.png)

![Screenshot from 2021-12-01 20-31-52](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-01%2020-31-52.png)



用户-服务器状态：cookies

大多数主要的门户网站使用cookies，cookies有四个组成部分：

- 在HTTP响应报文中有一个cookie的首部行
- 在HTTP请求报文中含有一个cookie的首部行
- 在用户端系统中保留有一个cookie文件，由用户的浏览器管理
- 在Web站点有一个后端数据库 

![Screenshot from 2021-12-01 20-43-54](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-01%2020-43-54.png)

Cookies能带来什么：用户验证、购物车、推荐、用户状态等

如何维护状态：

- 协议端节点：在多个事务上，发送端和接收端维持状态
- cookies：http报文携带状态信息



Web缓存（代理服务器）

目标：不访问原始服务器，就满足客户的需求

- 用户设置浏览器：通过缓存访问Web
- 浏览器将所有的HTTP请求发给缓存
  - 查缓存中的对象：缓存直接返回对象
  - 如对象不在缓存，缓存请求原始服务器，然后再将对象返回给客户端

- 缓存既是客户端又是服务器
- 通常缓存是由ISP安装（大学、公司、居民区ISP）
- 为什么要用Web缓存？
  - 降低客户端的请求响应时间
  - 可以大大减少一个机构内部网络与Internet接入链路上的流量
  - 互联网大量采用了缓存，可以使较弱的ICP也能够有效提供内容  



## 2.3 FTP（比较古老）

![Screenshot from 2021-12-02 22-02-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-02%2022-02-18.png)



- 向远程主机上传输文件或从远程主机接收文件
- 客户/服务器模式
  - 客户端：发起传输的一方
  - 服务器：远程主机
- ftp: RFC 959
- ftp服务器：端口号为21



FTP：控制连接与数据连接分开

- 控制连接
  - FTP客户端与FTP服务器通过端口21联系，并使用TCP作为传输协议
  - 客户端通过控制连接获得身份确认
  - 客户端通过控制连接发送命令浏览远程目录
  - 收到一个文件传输命令时，服务器打开一个客户端的数据连接
  - 一个文件传输完成后，服务器关闭连接
- 数据连接
  - 服务器打开第二个TCP数据连接用来传输另一个文件
  - 控制连接：带外（out of band）传送
  - FTP服务器维护用户的状态信息：当前路径、用户账户与控制连接对应（有状态的）



## 2.4 Email

3个组成部分

- 用户代理
  - 也就是软件，如outlook等等
- 邮件服务器
  - 邮箱中管理和维护发送给用户的邮件
  - 输出报文队列保持待发送邮件报文
  - 邮件服务器之间的SMTP协议：发送email报文
    - 客户：发送方邮件服务器
    - 服务器：接收端邮件服务器
- 简单邮件传输协议：SMTP

![Screenshot from 2021-12-02 22-39-08](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-02%2022-39-08.png)

- 邮件访问协议：从服务器访问邮件
  - POP3
  - IMAP
  - HTTP

![Screenshot from 2021-12-02 22-47-19](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-02%2022-47-19.png)



## 2.5 DNS (Domain Name System)域名解析系统

 DNS的必要性

- IP地址标识主机、路由器
- IP地址不好记忆
- 人类倾向于使用有意义的字符来标识Internet的设备
- 存在字符串-IP地址的转换的必要性
- DNS负责将人类用户提供的机器的字符串名称转换为二进制的网络地址

DNS总体思路和目标

- DNS的主要思路：
  - 分层的、基于域的命名机制
  - 若干分布式的数据库完成名字到IP地址的转换
  - 运行在UDP之上的53号端口的应用服务
  - 核心的Internet功能，但以应用协议实现
- DNS的主要目的：
  - 实现主机名-IP地址的转换
  - 其他目的
    - 主机别名到规范名字的转换：Host aliasing
    - 邮件服务器别名到邮件服务器正规名字的转换：Mail server aliasing
    - 负载均衡：Load Distribution

DNS系统需要解决的问题

- 问题1：如何命名设备
  - 解决一个平面命名的重名问题：层次化命名
- 问题2：如何完成名字到IP地址的转换
  - 分布式的数据库维护和响应名字查询
- 问题3：如何维护——增加或删除一个域，需要在域名系统中做哪些工作

### 问题1

DNS域名结构：

- 一个层面命名设备会有很多重名
- DNS采用层次树状结构的命名方法
- Internet根被划分为几百个顶级域（top lever domains）
  - 通用的(generic): .com; .edu; .gov; .net; .org
  - 国家的(countries): .cn; .us; .jp;
- 每个（子）域下面可划分为若干子域（subdomains）
- 树叶是主机



DNS根名字服务器：共有13个



域名

- 从本域往上，直到树根
- 域的域名：可以用于表示一个域
- 主机的域名，一个域上的一个主机

 域名的管理

- 一个域管理其下的子域
- 创建一个新的域，必须征得它所属域的同意

域与物理网络无关

- 域遵从组织界限，而不是物理网络
  - 一个域的主机可以不在一个网络
  - 一个网络的主机不一定在一个域
- 域的划分是逻辑的，而不是物理的



### 问题2

一个名字服务器的问题

- 可靠性问题：单点故障
- 扩展性问题：通信容量
- 维护问题：远距离的集中式数据库

区域（zone）

- 区域的划分由区域管理者自己决定
- 将DNS名字空间划分为互不相交的区域，每个区域都是树的一部分
- 名字服务器：
  - 每个区域都有一个名字服务器，维护着它所管辖区域的权威信息
  - 名字服务器允许被放置在区域之外，以保障可靠性



权威DNS服务器：组织结构的DNS服务器，提供组织结构服务器可访问的主机和IP之间的映射

组织结构可以选择实现自己维护或由某个服务提供商来维护





顶级域（TLD）服务器：负责顶级域名（com，org，net，edu和gov等）和所有国家级的顶级域名（cn，en，ca，jp）

- Network solutions公司维护com TLD服务器
- Educause公司维护edu TLD服务器



区域名字服务器维护资源记录

- 资源记录
  - 作用：维护域名-IP地址的映射关系
  - 位置：Name Server的分布式数据库中
- RR格式（domain_name, ttl, type, class, Value）
  - Domain_name：域名
  - Ttl：time to live 生存时间
  - Class：类别，对Internet来说值为IN
  - Value：可以是数字，域名或ASCII串
  - Type：资源记录的类型
    - Type = A：Name为主机，Value为IP地址
    - Type = CNAME：Name为规范名字的别名，Value为规范名字
    - Type = NS：Name为域名，Value为该域名的权威服务器的域名
    - Type = MX：Value为name对应的邮件服务器的名字

DNS大致工作过程

- 应用调用 解析器（resolver）
- 解析器作为客户向Name Server发出查询报文（封装在UDP段中）
- Name Server返回相应报文（name/ip）



本地名字服务器（Local Name Server）

- 并不严格属于层次结构
- 每个ISP都有一个本地DNS服务器
- 当一个主机发起一个DNS查询时，查询被送到其本地DNS服务器
  - 起着代理的作用，将查询转发到层次结构中

递归查询 & 迭代查询



DNS协议：查询和响应的报文格式相同

- 报文首部：标识符（ID）16位、flags（区别是查询还是应答等）



### 问题3：维护问题——新增一个域

- 在上级域的名字服务器中增加两条记录，指向这个新增的子域的域名 和 域名服务器的地址
- 在新增子域的名字服务器上运行名字服务器，负责本域的名字解析：名字->IP地址
- 例子：在com域中建立一个“Network Utopia”
  - 到注册登记机构注册域名networkutopia.com
    - 需要向该机构提供权威DNS服务器的名字和IP地址
    - 登记机构在com TLD服务器中插入两条RR记录
  - 在networkutopia.com的权威服务器中确保有
    - 用于web服务器的www.networkutopia.com 的类型为A的记录
    - 用于邮件服务器mail.networkutopia.com的类型为MX的记录 

![Screenshot from 2021-12-06 20-54-31](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-06%2020-54-31.png)





## 2.6 P2P应用

纯P2P架构

- 没有（或极少）一直运行的服务器
- 任意端系统都可以直接通信
- 利用peer的服务能力
- Peer节点间歇上网，每次IP地址都可能变化

P2P相比CS的优势

- 服务器多

P2P相比CS的缺点

- 难以管理

P2P文件共享

- 两大问题
  - 如何定位所需资源
  - 如何处理对等方的加入和离开
- 可能的方案
  - 集中
  - 分散
  - 半分散

P2P的模式

- 非结构化P2P：任意两节点之间都可以互通
  - 集中化目录（如Napster）
    - 当对等方连接时，它告知中心服务器IP地址及内容；下线时也要告知中心服务器
    - 查询定位是集中式的，文件分发是P2P的
    - 问题：单点故障、性能瓶颈、侵犯版权
  - 完全分布式（如Gnutella）
    - 没有中心服务器
    - 开方文件共享协议
    - 查询：向所有邻居发送消息，所有邻居也继续给其所有邻居发送（泛洪）；拥有该资源的节点反向发回
    - 如何建立这样一个网络：每个Gnutella客户端在安装的时候都有一个配置文件，配置了“死党”列表，加入之后ping死党，死党转发，收到ping信号之后给出相应。客户端收到响应后随机选择几个作为邻居节点。
    - 问题：维护起来比较困难
  - 混合体（如KaZaA）
    - 每个对等方要么是一个组长，要么隶属于一个组长
      - 对等方与组长之间有TCP链接
      - 组长对之间有TCP链接
    - 组长跟踪其所有的孩子的内容
    - 组长与其他组长联系：转发查询到其他组长，获得其他组长的数据拷贝
- DHT（结构化）P2P：节点之间的连接可以构成一个树状或者环装
  - 哈希表
  - DHT方案
  - 环形DHT以及覆盖网络
  - Peer波动



## 2.7 CDN

视频流化服务和CDN：上下文

多媒体：视频

- CBR(constant bit rate): 以固定速率编码
- VBR(variable bit rate): 视频编码速率随时间的变化而变化

 多媒体流化服务：DASH

- Dynamic, Adaptive Streaming over HTTP
- 服务器
  - 将视频文件分成多个块
  - 每个块独立存储，编码于不同码率
  - 告示文件（manifest file）：提供不同块的URL
- 客户端
  - 先获取告示文件
  - 周期性地测量服务器到客户端的带宽
  - 查询告示文件，在一个时刻请求一个块，HTTP头部指定字节范围
    - 如果带宽足够，选择最大码率的视频块
    - 会话中的不同时刻，可以切换请求不同的编码块（取决于当时的可用带宽）

Content Distribution Networks

- 挑战：服务器如何通过网络向上百万用户同时流化视频内容
- 选择1：单个的，大的超级服务中心
  - 服务器到客户端路径上跳数较多
  - 网络流量重复性高，效率低
  - 单点故障点，性能瓶颈
  - 周边网络拥塞
  - 简单，但是不可扩展
- 选择2：CDN，全网部署缓存节点，存储服务内容，就近为用户提供服务
  - enter deep：将CDN服务器深入到许多接入网
    - 更接近用户，数量多，离用户近，管理困难
  - bring home：部署在少数关键位置，如将服务器簇安装于POP附近
  - 工作流程：
    - 在CDN节点中存储内容的多个拷贝
    - 用户从CDN中请求内容 
  - “over the top”互联网主机-主机之间的通信作为一种服务向用户提供



## 2.8 TCP套接字编程

Socket编程

2种传输层服务的socket类型：

- TCP：可靠地、字节流的服务
- UDP：不可靠（数据UDP数据报）服务

TCP套接字编程

- 套接字：应用进程与端到端传输协议（TCP或UDP）之间的门户
- TCP服务：从一个进程向另一个进程可靠地传输字节流

工作流程：

- 服务器先运行，等到建立连接
  - 创建欢迎socket
  - 和本地端口捆绑
  - 在欢迎socket上阻塞式等待接收用户的链接
- 客户端主动和服务器建立链接
  - 创建客户端本地套接字
    - 指定服务器进程的IP地址和端口号，与服务器进程链接
  - 当与客户端连接请求到来时
    - 服务器接受来自用户端的请求，接触阻塞式等待，返回一个新的socket（与欢迎socket不同），与客户端通信
      - 允许服务器与多个客户端通信
      - 使用源IP和源端口来区分不同的客户端
  - 连接API调用有效时，客户端与服务器建立了TCP连接

![Screenshot from 2021-12-13 22-30-22](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-30-22.png)

![Screenshot from 2021-12-13 22-30-51](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-30-51.png)

![Screenshot from 2021-12-13 22-30-59](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-30-59.png)

![Screenshot from 2021-12-13 22-31-23](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-31-23.png)

![Screenshot from 2021-12-13 22-36-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-36-46.png)

![Screenshot from 2021-12-13 22-38-21](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-38-21.png)

## 2.9 UDP套接字编程

UDP：在客户端和服务器之间没有连接

- 没有握手
- 发送端在每一个报文中明确地指定目标的IP地址和端口号
- 服务器必须从收到的分组中提取出发送端的IP地址和端口号

![Screenshot from 2021-12-13 22-44-22](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-13%2022-44-22.png)

## 小结

应用程序体系结构

- 客户-服务器
- P2P
- 混合

应用程序需要的服务品质描述

- 可靠性、带宽、延时、安全

Internet传输层服务模式

- 可靠地、面向连接的服务：TCP
- 不可靠的数据报：UDP

流行的应用层协议

- HTTP
- FTP
- SMTP, POP, IMAP
- DNS

Socket编程

协议相关知识：一个协议定义了在两个或多个通信实体之间交换报文的格式和次序、以及就一条报文传输和接收或其他事件采取的动作

- 应用层协议报文类型：请求/响应报文
  - 客户端请求信息或服务
  - 服务器以数据、状态码进行响应
- 报文格式
  - 首部：关于数据信息的字段
  - 数据：被交换的信息
- 控制报文 vs 数据报文：带内、带外？
- 集中式 vs 分散式
- 无状态 vs 维护状态
- 可靠地 vs 不可靠的报文传输
- 在网络边缘处理复杂性



# Chapter3: 传输层

目标：

- 理解传输层的工作原理
  - 多路复用/解复用
  - 可靠数据传输（RDT，reliable data transfer）
  - 流量控制
  - 拥塞控制
- 学习Internet的传输层协议
  - UDP：无连接传输
  - TCP：面向连接的可靠传输
  - TCP的拥塞控制

## 3.1 概述和传输层服务

传输服务和协议

- 为运行在不同主机上的应用进程提供逻辑通信
- 传输协议运行在端系统
  - 发送方：将应用层的报文分成报文段，然后传递给网络层
  - 接收方：将报文段重组成报文，然后传递给应用层
- 有多个传输层协议可供应用选择：Internet的TCP和UDP

传输层 vs. 网络层

- 网络层服务：主机之间的逻辑通信
- 传输层服务：进程间的逻辑通信
  - 依赖于网络层的服务：延时、带宽
  - 并对网络层的服务进行增强：数据丢失、顺序混乱、加密

Internet传输层协议

- 可靠的、保序的传输：TCP
  - 多路复用、解复用
  - 拥塞控制
  - 流量控制
  - 建立连接
- 不可靠、不保序的传输：UDP
  - 多路复用、解复用
  - 没有为尽力而为的IP服务添加更多的其它额外服务
- 都不提供的服务：
  - 延时保证
  - 带宽保证



## 3.2 多路复用与解复用

在发送方主机多路复用：从多个套接字接收来自多个进程的报文，根据套接字对应的IP地址和端口号等信息对报文段用头部加以封装（该头部信息用于以后的解复用）

在接收方主机多路解复用：根据报文段的同步信息中的IP地址和端口号将接收到的报文段发给正确的套接字和对应的应用进程

**实际就是IP+端口号区分不同的APP，以实现复用**



## 3.3 无连接传输：UDP (User Data Protocol)

- "no frills" "bare bones" Internet传输协议
- “尽力而为”的服务，报文段可能丢失或乱序
- 无连接
  - UDP发送端和接收端之间没有握手
  - 每个UDP报文段都被独立地处理
- UDP被用于：流媒体、DNS、SNMP
- 在UDP上实现可靠传输：
  - 在应用层增加可靠性
  - 应用特定的差错恢复

![Screenshot from 2021-12-14 20-13-07](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-14%2020-13-07.png)

UDP校验和（EDC）：

- 目标：检测在被传输报文段中的差错
- 发送方：
  - 将报文段的内容视为16比特的整数
  - 校验和：报文段的加法和（1的补运算）
  - 发送方将校验和放在UDP的校验和字段
- 接收方：
  - 计算接收到的报文段和校验和
  - 检查计算出的校验和与校验和字段的内容是否相等：
    - 不相等——检测到差错
    - 相等——没有检测到差错，但也许还是有差错（残存错误）

![Screenshot from 2021-12-14 20-20-11](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-14%2020-20-11.png)





## 3.4 可靠数据传输(rdt)的原理

rdt在应用层、传输层和数据链路层都很重要，是网络Top10问题之一 。

![Screenshot from 2021-12-15 19-55-50](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2019-55-50.png)

我们将：

- 渐增式地开发可靠数据传输协议的发送方和接收方
- 只考虑单项数据传输（但控制信息是双向流动的）
- 双向的数据传输问题实际上是2个单项数据传输问题的综合
- 使用有限状态机（FSM）来描述发送方和接收方



Rdt1.0：在可靠信道上的可靠数据传输

- 下层的信道是完全可靠的
  - 没有比特出错
  - 没有分组丢失
- 发送方和接收方的FSM
  - 发送方将数据发送到下层通道
  - 接收方从下层信道接收数据

Rdt2.0：具有比特差错的信道

- 下层信道可能会出错：将分组中的比特翻转
  - 用校验和来检测比特差错
- 问题：怎样从差错中恢复
  - 确认（ACK）：接收方显式地告诉发送方分组已被正确接收
  - 否定确认（NAK）：接收方显式地告诉发送方分组发生了差错。发送方收到NAK后，重传分组。
- rdt2.0中的新机制：采用差错控制编码进行差错检测
  - 发送方差错控制编码、缓存
  - 接收方使用编码检错
  - 接收方得反馈：控制报文（ACK，NAK）
  - 发送方收到反馈相应的动作

![Screenshot from 2021-12-15 20-10-29](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2020-10-29.png)

rdt2.0的致命缺陷：ACK/NAK传输过程中出错，发送方不知道接收方发的是啥->rdt2.1 处理重复，停止等待stop and wait

- 发送方在每个分组中加入序号
- 如果ACK/NAK出错，发送方重传当前分组
- 接收方丢弃重复分组

![Screenshot from 2021-12-15 20-17-17](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2020-17-17.png)

![Screenshot from 2021-12-15 20-18-27](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2020-18-27.png)

rdt2.2：无NAK的协议

- 功能同rdt2.1，但只使用ACK（ACK要编号）
- 接收方对最后正确接收的分组发ACK，以替代NAK
  - 接收方必须显式地包含被正确接收分组的序号
- 当收到重复的ACK（如再次收到ack0）时，发送方与收到NAK采取相同的动作，重传当前分组
- 为后面的一次发送多个数据单位做一个准备
  - 一次能够发送多个
  - 确认信息减少一半，协议处理简单

rdt3.0：具有比特差错和分组丢失的信道

- 新的假设：下层信道可能会丢失分组（数据或者ACK）
  - 会死锁（发送方等确认，接收方等数据，双方都在等，死锁，引入超时重传机制）
  - 机制还不够处理这种状况：校验和、序列号、ACK、重传

- 方法：发送方等待ACK一段合理的时间
  - 发送端超时重传：如果到时没有收到ACK->重传
  - 问题：如果分组（或ACK）只是被延迟了
    - 重传会导致数据重复，但利用序列号已经可以处理这个问题
    - 接收方必须指明被正确接收的序列号
  - 需要一个倒计数定时器

- rdt3.0的问题：在信道容量比较大的时候，信道利用率低，因为一次只发送一个分组，这是由于协议所导致的问题



流水线协议：

- 流水线：允许发送方在未得到对方确认的情况下一次发送多个分组
  - 必须增加序号的范围：用多个bit表示分组的序号
  - 在发送方/接收方要有缓冲区
    - 发送方缓冲：为得到确认，可能要重传
    - 接收方缓存：上层用户取用数据的速率≠接收到的数据；接收到的数据可能乱序，排序交付
- 两种通用的流水线协议：回退N步（GBN）和选择重传（SR）

通用：滑动窗口（slide window）协议

- 几种情况
  - sending window = 1, receiving window = 1, stop and wait
  - sw > 1, rw = 1, GBN
  - sw > 1. rw > 1, SR

-   发送缓冲区
  - 形式：内存中的一个区域，落入缓冲区的分组可以发送
  - 功能：用于存放已发送，但是没有得到确认的分组
  - 必要性：需要重发时可用
- 发送缓冲区的大小：一次最多可以发哦少年宫多少个未经确认的分组
  - 停止等待协议=1
  - 流水线协议>1，不能很大，链路利用率不能超过100%
- 发送缓冲区的分组
  - 未发送的：落入发送缓冲区的分组，可以连续发送出去
  - 已经发送出去的、等到对方确认的分组：发送缓冲区的分组只有得到确认才能删除
- 发送窗口：发送缓冲区内容的一个范围
  - 那些已发送但是未经确认分组的序号构成的空间
  - 发送窗口的最大值<=发送缓冲区的值
  - 一开始：没有发送任何一个分组
    - 后沿=前沿
    - 发送窗口的尺寸=0
  - 每发送一个分组，前沿前移一个单位
  - 前沿不能超过发送缓冲区
- 发送窗口的移动->后沿移动
  - 条件：收到老分组的确认
  - 结果：发送缓冲区罩住新的分组，来了分组可以发送
  - 极限：不超过前沿
- 接收窗口 = 接收缓冲区
  - 接受窗口用于控制哪些分组可以接收
    - 只有收到的分组序号落入接收窗口内才允许接收
    - 若在窗口外则丢弃
  - 接收窗口尺寸为1，则只能顺序接收
  - 接收窗口尺寸大于1，则可以乱序接收
- 接收窗口的滑动和发送确认
  - 滑动：
    - 低序号的分组到来，接收窗口移动
    - 高序号分组乱序到，缓存但不交付，不滑动
  - 发送确认：
    - 接收窗口尺寸为1，发送连续收到的最大的分组确认（累计确认）
    - 接收窗口尺寸大于1，发送所收到分组的确认（非累计确认）

![Screenshot from 2021-12-15 21-30-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2021-30-43.png)

![Screenshot from 2021-12-15 21-34-09](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-15%2021-34-09.png)



## 3.5 面向连接的传输：TCP

概述

- 点对点
  - 一个发送方，一个接收方
- 可靠的、按顺序的字节流
  - 没有报文边界
- 管道化（流水线）
  - TCP拥塞控制和流量控制设置窗口大小
- 发送和接收缓存
- 全双工数据
  - 在同一连接中数据流双向流动
  - MSS：最大报文段大小
- 面向连接
  - 在数据交换之前，通过握手（交换控制报文）初始化发送方、接收方的状态变量
- 有流量控制
  - 发送方不会淹没接收方

![Screenshot from 2021-12-16 19-55-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2019-55-00.png)

- 序号：报文段首字节的在字节流的编号
- 确认号：期望从另一方收到的下一个字节的序号、累积确认

TCP往返延时（RTT）和超时

- 怎样设置TCP超时
  - 比RTT药厂
  - 太短->过早超时导致不必要重传；太长->对报文段丢失反应太慢，消极
- 怎样估计RTT
  - SampleRTT：测量从报文段发出到收到确认的时间，如果重传则忽略此次测量
  - SampleRTT会变化，因此估计的RTT应该比较平滑，对几个最近的测量值求平均

![Screenshot from 2021-12-16 20-38-46](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2020-38-46.png)

**可靠数据传输**

- TCP在IP不可靠服务的基础上建立了RDT
  - 管道化的报文段 GBN or SR
  - 累积确认
  - 单个重传定时器
  - 是否可以接受乱序的，没有规范
- 通过以下事件触发重传
  - 超时
  - 重复确认
- 首先考虑简化的TCP发送方
  - 忽略重复的确认
  - 忽略流量控制和拥塞控制

![Screenshot from 2021-12-16 21-08-24](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2021-08-24.png)

![Screenshot from 2021-12-16 21-08-32](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2021-08-32.png)

**流量控制**

接收方控制发送方，不让发送方发送的太多，太快以至于让接收方的缓冲区溢出

缓冲区TCP往里面写，app从当中读取

- 接收方在其向发送方的TCP段头部的rwnd字段“通告”其空闲buffer大小
  - RcvBuffer大小通过socket选项设置
- 发送方限制未确认（“in flight”）字节的个数≤接收方发过来的rwnd值
- 保证接收方不会被淹没

**连接管理**

在正式交换数据之前，发送方和接收方握手监理通信关系

- 同意建立连接
- 同意连接参数

为什么不能2次握手？

- 变化的延迟
- 由于丢失造成的重传
- 报文乱序
- 相互看不到对方

![Screenshot from 2021-12-16 21-27-20](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2021-27-20.png)



![Screenshot from 2021-12-16 21-30-15](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2021-30-15.png)



**关闭连接**

- 客户端，服务器分别关闭他自己这一侧的连接
  - 发送FIN bit = 1的TCP段
- 一旦接受到FIN，用ACK回应
  - 接收到FIN段，ACK可以和它自己发出的FIN段一起发送
- 可以处理同时的FIN交换

![Screenshot from 2021-12-16 21-36-58](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-16%2021-36-58.png)



## 3.6 拥塞控制原理

拥塞：

- 非正式的定义：“太多的数据需要网络传输，超过了网络的处理能力”
- 与流量控制不同
- 拥塞的表现
  - 分组丢失（路由器缓冲区溢出）
  - 分组经历比较长的延迟（在路由器的队列中排队）

拥塞的原因/代价：场景1

- 2个发送端，2个接收端
- 一个路由器，具备无限大的缓冲
- 输出链路带宽：R
- 没有重传

 拥塞的原因/代价：场景2

- 一个路由器，有限的缓冲
- 分组丢失时，发送端重传
  - 应用层的输入 = 应用层的输出
  - 传输层的输入包括重传

 拥塞的原因/代价：场景3

- 死锁

![Screenshot from 2021-12-19 19-01-58](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-19%2019-01-58.png)

拥塞的代价：

- 为了达到一个有效输出，网络需要做更多的工作（重传）
- 没有必要的重传，链路中包括了多个分组的拷贝
  - 是那些没有丢失，经历的时间比较长（拥塞状态）但是超时的分组
  - 降低了的“goodput”
- 当分组丢失时，任何“关于这个分组的上游传输能力”都被浪费了

拥塞控制方法

- 端到端拥塞控制
  - 没有来自网络的显式反馈
  - 端系统根据延迟和丢失事件推到是否有拥塞
  - TCP采用的方法
- 网络辅助的拥塞控制
  - 路由器提供给端系统以反馈信息
    - 单个bit置位，显示有拥塞（SNA，DECbit，TCP/IP ECN，ATM）
    - 显式提供发送端可以采用的速率

![Screenshot from 2021-12-19 19-09-10](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-19%2019-09-10.png)

![Screenshot from 2021-12-19 19-13-12](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-19%2019-13-12.png)



## 3.7 TCP拥塞控制

端到端的拥塞控制

- 机制
  - 路由器不向主机发送有关拥塞的反馈信息
    - 路由器的负担较轻
    - 符合网络核心简单的TCP/IP构架原则
  - 端系统根据自身得到的信息，判断是否发生拥塞，从而采取动作
- 问题
  - 如何检测拥塞：轻微拥塞、拥塞
  - 控制策略
    - 在拥塞发生时如何动作，降低速率：轻微拥塞、拥塞
    - 在拥塞缓解时如何动作，增加速率

拥塞感知：发送端如何检测到拥塞？

- 某个段超时了（丢失事件）：拥塞
- 有关某个段的3次重复ACK：轻微拥塞

速率控制方法：如何控制发送端发送的速率

- 维持一个拥塞窗口的值：CongWin
- 发送端限制已发送但是未确认的数据量（的上限）：LastByteSent-LastByteAcked <= CongWin
- 从而粗略地控制发送方往网络中注入的速率
- CongWin是动态的，是感知到的网络拥塞程度的函数
  - 超时或者3个重复的ack，CongWin↓
    - 超时时：CongWin降低为1MSS，进入SS（慢启动）阶段然后再倍增到CongWin/2（每个RTT），从而进入CA阶段
    - 3个重复ack：CongWin降为CongWin/2，CA阶段
  - 否则（正常收到ACK，没有发生以上情况）：CongWin↑
    - SS阶段：加倍增加（每个RTT）
    - CA阶段：线性增加（每个RTT）

TCP拥塞控制和流量控制的联合动作

联合控制的方法：

- 发送端控制发送但是未确认的量同时也不能够超过接收窗口，满足流量控制需求
  - SendWin = min{CongWin, RecvWin}
  - 同时满足拥塞控制和流量控制要求

TCP拥塞控制策略概述

- 慢启动

![Screenshot from 2021-12-20 22-34-30](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-34-30.png)

![Screenshot from 2021-12-20 22-34-52](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-34-52.png)

- AIMD：线性增，乘性减

  ![Screenshot from 2021-12-20 22-38-53](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-38-53.png)

  ![Screenshot from 2021-12-20 22-37-40](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-37-40.png)

- 超时事件后的保守策略

总结：TCP拥塞控制

- 当CongWin<Threshold，发送端处于慢启动阶段，窗口指数性增长
- 当CongWin>Threshold，发送端处于拥塞避免状态，窗口线性增长
- 当收到三个重复的ACKs，Threshold设置成CongWin/2，CongWin=Threshold+3
- 当超时事件发生时timeout，Threshold=CongWin/2，CongWin=1MSS，进入SS阶段

![Screenshot from 2021-12-20 22-43-38](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-43-38.png)



TCP公平性目标：如果K个TCP会话分享一个链路带宽为R的瓶颈，每一个会话的有效带宽为R/K

![Screenshot from 2021-12-20 22-48-03](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-48-03.png)

![Screenshot from 2021-12-20 22-50-37](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-50-37.png)

![Screenshot from 2021-12-20 22-51-32](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-51-32.png)

![Screenshot from 2021-12-20 22-52-49](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-20%2022-52-49.png)



# Chapter4：网络层 数据平面

## 4.1 导论

网络层服务

- 在发送主机和接收主机之间传送段
- 在发送端将段封装到数据报中
- 在接收端，将段上交给传输层实体
- 网络层协议存在于每一个主机和路由器
- 路由器检查每一个经过它的IP数据报的头部

网络层功能：

- 转发：将分组从路由器的输入接口转发到合适的输出接口
- 路由：使用路由算法来决定分组从发送主机到目标接收主机的路径
  - 路由选择算法
  - 路由选择协议

数据平面

- 本地、每个路由器功能
- 决定从路由器输入端口到达的分组如何转发到输出端口
- 转发功能
  - 传统方式：基于目标地址+转发表
  - SDN方式：基于多个字段+流表

控制平面

- 网路范围内的逻辑
- 决定数据报如何在路由器之间路由，决定数据包从源到目标主机之间的端到端路径
- 2个控制平面方法
  - 传统的路由算法：在路由器中被实现
  - Software-defined networking(SDN): 在远程的服务器中实现

![Screenshot from 2021-12-21 21-59-23](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-21%2021-59-23.png)

![Screenshot from 2021-12-21 22-00-08](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-21%2022-00-08.png)

![Screenshot from 2021-12-21 22-12-16](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-21%2022-12-16.png)

网络服务模型

- 对于单个数据报的服务
  - 可靠传送
  - 延迟保证
- 对于数据报流的服务
  - 保序数据报传送
  - 保证流的最小带宽
  - 分组之间的延迟差

连接建立

- 在某些网络架构中是第三个重要的功能（在路由和转发之后）
- 在分组传输之前，在两个主机之间，在通过一些路由器所构成的路径上建立一个网络层连接
  - 涉及到路由器
- 网络层和传输层连接服务区别：
  - 网络层：在2个主机之间，涉及到路径上的一些路由器
  - 传输层：在2个进程之间，很可能只体现在端系统上（TCP连接）



![Screenshot from 2021-12-21 22-19-32](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-21%2022-19-32.png)

 

## 4.2 路由器组成

![Screenshot from 2021-12-27 19-21-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-21-28.png)

![Screenshot from 2021-12-27 19-22-50](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-22-50.png)

![Screenshot from 2021-12-27 19-35-50](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-35-50.png)

![Screenshot from 2021-12-27 19-36-09](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-36-09.png)

![Screenshot from 2021-12-27 19-39-07](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-39-07.png)

![Screenshot from 2021-12-27 19-40-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-40-48.png) 

![Screenshot from 2021-12-27 19-41-55](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-41-55.png)

![Screenshot from 2021-12-27 19-48-18](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-48-18.png)

![Screenshot from 2021-12-27 19-48-28](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-48-28.png)

![Screenshot from 2021-12-27 19-48-35](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-48-35.png)

![Screenshot from 2021-12-27 19-48-58](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-48-58.png)

![Screenshot from 2021-12-27 19-49-37](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-49-37.png)

![Screenshot from 2021-12-27 19-51-43](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-51-43.png)

![Screenshot from 2021-12-27 19-51-48](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-27%2019-51-48.png)

## 4.3 IP: Internet Protocol

![Screenshot from 2021-12-29 12-48-00](https://raw.githubusercontent.com/Lbaron980810/blog_img/main/ComputerNet/Screenshot%20from%202021-12-29%2012-48-00.png)

IP协议：实现数据平面的转发功能



























# Bottom









































