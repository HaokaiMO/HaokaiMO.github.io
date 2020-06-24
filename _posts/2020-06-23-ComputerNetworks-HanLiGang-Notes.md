---
title: '韩立刚老师讲计算机网络--笔记'
category: Computer Networks
tags:
  - 'Computer Networks'
---

这份笔记是我为了回顾下计算机网络方面的知识， 2020 年 2 月中旬在 B 站上看韩立刚老师讲计算机网络视频课程时所记，视频课程中韩老师以谢希仁编写的[《计算机网络-第 5 版》](https://book.douban.com/subject/2970300/)教材为基础，在讲完一些章节的知识点后还会在电脑上用一些工具来进行实验讲解，效果个人感觉比一些大学课堂上老师讲计算机网络时，只讲原理，念下 PPT 上的内容要生动些。

<!-- more -->

我看的这套视频共 156 集，每集时长十几分钟到几十分钟不等。在看的过程中我发现这套视频录制的时间在 2014 年 5 月 27 号左右，不过在视频中的一些实验操作部分有看到日期显示为 2013 年的某月，总的来说视频录制时间是在 2013-2014 年样子。



&nbsp;

## 01 计算机网络概述

1.1 计算机网络在信息时代的作用



1.2 因特网概述

因特网：

- 网络：许多计算机连接在一起；
- 互联网：许多网络连接在一起；
- 因特网：Internet 全球最大的一个互联网；

多层次 ISP（Internet Service Provider）结构的互联网



1.3 因特网的组成

因特网的核心部分

数据交换方式：电路交换（Circuit Switching）；报文交换（Message Switching）；**分组交换（Packet Switching）**。

因特网的边缘部分：

主机之间的通信方式：客户服务器方式（Client/Server方式，C/S）；对等方式（Peer-to-Peer方式，P2P）。



1.4 计算机网络在我国的发展



1.5 计算机网络的类别

不单单从网络覆盖范围区分局域网和广域网，还要看应用了什么技术（广域网技术、局域网技术）。

局域网：自己购买设备，自己维护，带宽固定，比如 100 M 或 1000 M 交换机，距离 100 米以内。

广域网：花钱买服务，花钱买带宽。



1.6 计算机网络的性能

性能指标：1）速率；2）带宽；3）吞吐量；4）时延（发送时延，传播时延，排队时延，处理时延）；5）时延带宽积；6）往返时间（RTT, Round-Trip-Time；从发送方发送数据开始，到发送方收到接收方的确认。）；7）利用率；

非性能指标：1）费用；2）质量；3）标准化；4）可靠性；5）可扩展性；6）可升级性；7）管理与维护；



1.7 计算机网络的体系结构

分层的意义：标准化；降低相互之间的影响。

网络排错：从网络的底层到高层进行排错。

实验部分：

- 实验1-1：使用 ipconfig 查看本机 IP 地址
- 实验1-2：使用 ping 测试网络连通性
- 实验1-3：使用防火墙保护系统的安全
- 扩展实验1-1：使用 netstat 命令查看木马
- 扩展实验1-2：UNC 资源共享



## 02 物理层

奈氏准则（码元传输速率受奈氏准则的限制）与香农公式（信息传输速率受香农公式的限制，存在噪声）的应用范围。



## 03 数据链路层

指引：

- 数据链路层基本概念及基本问题
  - 基本概念
  - 三个基本问题

- 两种情况下的数据链路层
  - 使用点对点信道的数据链路层
  - 使用广播信道的数据链路层

- 以太局域网（以太网）
  - 概述
  - 拓扑
  - 信道利用率
  - MAC层

- 扩展以太网

- 高速以太网



数据链路层使用的信道主要有两种：点对点信道；广播信道；

数据链路层要解决的三个基本问题：封装成帧；透明传输；差错控制；

CRC (Cyclic Redundancy Check) 差错检测技术

PPP 协议：现在全世界使用得最多的数据链路层协议是点对点协议（PPP，Point-to-Point Protocol）。用户使用拨号电话线接入因特网时，一般都是使用 PPP 协议。



以太网使用 CSMA/CD 协议（载波监听多点接入、碰撞检测，Carrier Sense Multiple Access with Collision Detection）

**使用 CSMA/CD 协议的以太网不能进行全双工通信，而只能进行半双工通信**。每个站在发送数据之后的一小段时间内，存在着遭遇碰撞的可能性。这种发送的不确定性使整个以太网的平均通信量远小于以太网的最高数据率。

最先发送数据帧的端，在发送数据帧至多经过时间 2τ（两倍的端到端往返时延）就可知道发送的数据帧是否遭受了碰撞。经过争用期这段时间还没有检测到碰撞，才能肯定这次发送不会发生碰撞。

最短有效帧长。凡长度小于这个字节长度的帧都是由于冲突而异常中止的无效帧。

二进制指数类型退避算法。



以太网的两个标准：DIX Ethernet V2（世界上第一个局域网产品（以太网）的规约）；IEEE 的 802.3 标准。DIX Ethernet V2 标准与 IEEE 的 802.3 标准只有很小的差别，因此可以将 802.3 局域网称为 “以太网”。严格来说，“以太网” 应当指符合 DIX Ethernet V2 标准的局域网。

以太网与数据链路层的两个子层：为了使数据链路层能更好地适应多种局域网标准，802 委员会就将局域网的数据链路层拆成两个子层：逻辑链路控制层 LLC（Logical Link Control）子层；媒体接入控制 MAC（Medium Access Control）子层。

由于 TCP/IP 体系经常使用的局域网是 DIX Ethernet V2，而不是 802.3 标准中的几种局域网，因此现在 802 委员会制定的逻辑链路控制子层 LLC（即 802.2 标准）的作用已经不大了。很多厂商生产的适配器上就仅装有 MAC 协议而没有 LLC 协议。

**以太网提供的服务是不可靠的交付，即尽最大努力的交付**。当接收端收到有差错的数据帧时就丢弃此帧，其他什么也不做。差错的纠正由网络的高层来决定。如果高层发现丢失了一些数据而进行重传，但是以太网并不知道这是一个重传的帧，而是当作一个新的数据帧来发送。

对以太网的参数要求：**当速率一定时，以太网的连线长度受到限制，否则 τ（端到端的时延） 的数值会太大；以太网的帧长不能太短，否则 T0（帧的发送时间） 的值会太小，使参数 a（a = τ/T0） 值太大**。



在局域网中，硬件地址又称为物理地址，或 MAC 地址。

- IEEE 的注册管理机构 RA 负责向厂家分配地址字段的前三个字节（即高位 24 位）。
- 地址字段中的后三个字节（即低位 24 位）由厂家自行指派，称为扩展标识符，必须保证生产出的适配器没有重复地址。
- 一个地址块可以生成 2^24 个不同的地址。这种 48 位地址称为 MAC-48，它的通用名称是 EUI-48。
- **MAC 地址实际上就是适配器地址或适配器标识符 EUI-48**。



**用集线器连的网，建议最多不超过30台电脑，连的数量越多，电脑通信时越容易发生碰撞，效率越低**。

在数据链路层扩展局域网是使用网桥。网桥工作在数据链路层，它根据 MAC 帧的目的地址对收到的帧进行转发。网桥具有过滤帧的功能。当网桥收到一个帧时，并不是向所有的接口转发此帧，而是先检查此帧的目的 MAC 地址，然后再确定将该帧转发到哪一个接口。

**现在多用的是交换机，网桥用得不多了**。



扩展以太网：

LAN 和 VLAN

**交换机的使用使得 VLAN（虚拟局域网） 的创建成为可能。VLAN 是由一些局域网网段构成的与物理位置无关的逻辑组**。

- 这些网段具有某些共同的需求。
- 每个 VLAN 的帧都有一个明确的标识符，指明发送的这个帧的工作站是属于哪一个 VLAN。

**虚拟局域网其实只是局域网给用户提供的一种服务，而并不是一种新型局域网**。

一个 VLAN = 一个广播域 = 逻辑网段（子网）



高速以太网：

速率达到或超过 100 Mb/s 的以太网称为高速以太网。在双绞线上传送 100 Mb/s 基带信号的星型拓扑以太网，仍使用 IEEE 802.3 的 CSMA/CD 协议。

100BASE-T 以太网又称为快速以太网（Fast Ethernet）。

吉比特以太网



在交换机上实现数据链路层的安全（固定 MAC 地址）。



## 04 网络层

指引：

- 网络层提供的两种服务

- 网际协议 IP

- 网际控制报文协议 ICMP

- 因特网的路由选择协议

- IP 多播

- 虚拟专用网 VPN 和网络地址转换 NAT



网络层提供的两种服务：

网络层关注的是如何将分组从源端沿着网络路径送达目的端。

在计算机网络领域，网络层应该向运输层提供怎样的服务（“面向连接” 还是 “无连接”）曾引起了长期的争论。争论焦点的实质就是：在计算机通信中，可靠交付应当由谁来负责？是网络还是端系统？

两种服务：网络层应该向运输层提供怎样的服务？

- 虚电路服务
- 数据报服务

**实际网络层用的是数据报服务，曾经想用虚电路服务**。



网络互连的设备：

中间设备又称中间系统或中继（relay）系统

- 物理层中继系统：转发器（repeater）；（集线器，hub）
- 数据链路层中继系统：网桥或桥接器（bridge）；（或交换机）
- 网络层中继系统：路由器（router）；



网络层的四个协议：IP 协议；ARP 协议（地址解析协议，Address Resolution Protocol）；ICMP 协议（网际控制报文协议，Internet Control Message Protocol）；IGMP 协议（网际组管理协议，Internet Group Management Protocol）。

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/IPv4protocolstack.JPG" alt="IPv4protocolstack" style="zoom:45%;" />



层次化的 IP 地址：层次化的 IP 地址将 32 位的 IP 地址分为网络 ID 和主机 ID。

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/IPAddressTypes.JPG" alt="IPAddressTypes" style="zoom:50%;" />

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ABCDEaddressHeadNumbers.JPG" alt="ABCDEaddressHeadNumbers" style="zoom:60%;" />



常用的三种类别的 IP 地址：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/theRangeofThreeIPAddress.JPG" alt="theRangeofThreeIPAddress" style="zoom:60%;" />



**A 类地址 127 开头的这个网络号本地计算机保留。IPv4 中主机部分地址不能全为 0 和 1（十进制表示即不能为 0 和 255），全为 0 表示一个网络号，全为 1 表示一个广播**。

特殊的几个地址：

127.0.0.1 ，本地环回地址。

169.254.0.0 ，windows 系统临时给本地电脑分配的地址。

保留的私网地址（RFC1918 指明的专用地址，private address）：

- 10.0.0.0 到 10.255.255.255
- 172.16.0.0  -- 172.31.255.255
- 192.168.0.0 -- 192.168.255.255

**私网地址通过互联网访问不到。公网地址（全球唯一的，进行统一规划的地址）**。



点到点的子网掩码：255.255.255.252

一个网段划分子网时，子网掩码相应的往后移位，移一位划分成 2 个，移两位划分成 4 个，移三位划分成 8 个，... 。



超网：

如何让两个子网的计算机划分在一个网段呢？

要把 192.168.0.0 和 192.168.1.0 两个 C 类网络合并，将 IP 地址第 3 字节和第 4 字节写成二进制，可以看到将子网掩码左移 1 位，网络部分就一样了，这两个网段就在一个网段了，如下图所示：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/combineTwoSubnet.JPG" alt="combineTwoSubnet" style="zoom:55%;" />



计算机 A 和计算机 B 通信过程：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/IPAddressandMACAddressDuringTwoComputersCommunication.JPG" alt="IPAddressandMACAddressDuringTwoComputersCommunication" style="zoom:60%;" />



- **交换机基于数据帧的 MAC 地址转发数据帧，路由器基于数据包的 IP 地址转发数据包**。
- **数据包在传输过程不变，过网络设备数据帧要用新的物理层地址重新分装**。
- **MAC 地址决定了数据帧下一跳哪个设备接收，而 IP 地址决定了数据包的起点和终点**。



`ping 219.148.38.148 -i 5` （通过 -i 参数设置数据包中 TTL 的值为 5。通过 ping 命令中默认的数据包 TTL 值也可以判断出是什么操作系统。）



数据路由：路由器在不同网段转发数据包。

网络中数据包能去能回，那么网络就畅通了。

- 去：沿途的路由器必须知道**到目标网络下一跳给哪个接口**；
- 回：沿途的路由器必须知道**到源网络下一跳给哪个接口**。



静态路由。

Windows 网关就是默认路由。

`route add` （添加路由表）

`route print` 或 `netstat -r` （查看路由表）



ICMP 允许主机或路由器**报告差错情况和提供有关异常情况的报告**。ICMP 报文**作为 IP 层数据报的数据，加上数据报的首部，组成 IP 数据报发送出去**。

ICMP 报文的种类有两种，即 **ICMP 差错报告报文和 ICMP 询问报文**。

- 差错报告报文有五种：终点不可达；源点抑制（Source quench）；时间超过；参数问题；改变路由（重定向，Redirect）。
- 询问报文有两种：回送请求和回答报文，时间戳请求和回答报文。



PING 用来测试两个主机之间的连通性。PING 使用了 ICMP 回送请求与回送回答报文。**PING 是应用层直接使用网络层 ICMP 的例子，它没有通过运输层的 TCP 或 UDP**。

PING（Packet Internet Grope），因特网包探索器，用于测试网络连接量的程序。

`pathping 219.148.38.148`  能够跟踪数据包的路径，网络排错的时候有用。 



动态路由协议：RIP（Routing Information Protocol） 最早，周期性广播，30 秒，最大跳数 16 跳（**RIP 协议认为一条路径中跳数越少越优，但是它不考虑带宽**；RIP 协议是开放式的）。

**在咱们这本书中我发现它特别爱分析数据包的结构，这个我觉得意义不大**。



OSPF （Open Shortest Path First）：动态路由协议（开放式）；**最佳路径的度量值为带宽**；支持多区域；触发式更新；有三个表（邻居表（发 hello 数据包来确认）、链路状态表、计算路由表）



`tracert 172.17.0.2` 



**边界网关协议：BGP 是不同自治系统的路由器之间交换路由信息的协议。每个自治系统的管理员要选择至少一个路由器作为该自治系统的 “BGP 发言人”**。



`netstat -n` （查看已建立的 TCP 会话。看不到 UDP 协议，因为 UDP 协议不建立会话。）

`netstat -nb` （可以查看会话是由哪个进程产生的。）

Windows 下连接远程桌面：在运行窗口中输入 `mstsc` （microsoft terminal services client，微软终端服务客户端）



IGMP：组播 = 多播



## 05 传输层

应用层：http、https、ftp、DNS、SMTP、PoP3、RDP

传输层：TCP、UDP

网络层：IP (RIP  OSPF  BGP)、ICMP、IGMP、ARP



传输层两个协议应用场景：

TCP：分段；编号；流量控制；建立会话（ `netstat -n` ）；（QQ上传文件）

UDP：一个数据包就能完成数据通信；不建立会话；（DNS域名解析、QQ上发消息、多播）



传输层和应用层之间的关系：**应用层在传输层的基础上多了个端口**。

传输层为**相互通信的应用进程提供了逻辑通信**。网络层为**主机之间提供逻辑通信**。

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/PortNumberandApplicationLayerProtocol.JPG" alt="PortNumberandApplicationLayerProtocol" style="zoom:50%;" />

http = TCP + 80

https = TCP + 443

ftp = TCP + 21

SMTP = TCP + 25

POP3 = TCP + 110

RDP = TCP + 3389

共享文件夹 = TCP + 445

SQL = TCP + 1433

DNS = UDP + 53 or TCP + 53（少量情况下使用TCP协议）



应用层协议和服务之间的关系：服务（对外的服务）运行后在 TCP 或 UDP 的某个端口侦听客户端请求。

查看自己计算机侦听的端口：`netstat -an` 

查看建立的会话：`netstat -n` 

查看建立的会话的进程：`netstat -nb` 



测试远程计算机打开的端口：`telnet 10.7.1.53 21` （Win 10 的命令行上要执行 telnet 命令需要另外启动 Telnet Client 功能）

端口代表服务。

更改端口增加服务器安全。

Windows 防火墙的作用（服务器上只开必要的端口）

Windows 防火墙控制不了木马程序（灰鸽子木马），中了木马后计算机主动访问外面的计算机。



指引：

- 传输层的功能

- 传输层协议 UDP 和 TCP

- TCP 可靠传输的实现

- TCP 的流量控制

- TCP 的拥塞控制

- TCP 的传输连接管理



两个对等传输实体在通信时传送的数据单位叫做 **传输协议数据单元（Transport Protocol Data Unit）**。

TCP 传送的协议数据单元是 TCP 报文段（Segment）。

UDP 传送的协议数据单元是 UDP 报文或用户数据报。

UDP 在传送数据之前不需要先建立连接。对方的传输层在收到 UDP 报文后不需要给出任何确认。虽然 UDP 不提供可靠交付，但在某些情况下 UDP 是一种最有效的工作方式。

**TCP 则提供面向连接的服务。TCP 不提供广播或多播服务，由于TCP要提供可靠的、面向连接的运输服务，因此不可避免地增加了许多的开销**。



DNS 服务器地址：222.222.222.222（石家庄电信）；8.8.8.8（谷歌）；

在 Windows 命令行中输入  `hostname` 执行后显示主机名



三类端口：

- 熟知端口，数值一般为 0-1023 。
  - FTP：21
  - TELNET：23
  - SMTP：25
  - DNS：53
  - HTTP：80
  - HTTPS：443
  - RDP：3389

- 登记端口号，数值为 1024-49151 。

- 客户端口号，数值为 49152-65535 。 



`netstat -n | find "ESTABLISHED"` 



UDP 协议主要特点：

- UDP 是无连接的，即发送数据之前不需要建立连接。
- UDP 使用尽最大努力交付，即不保证可靠交付，同时也不使用拥塞控制。
- UDP 是面向报文的。UDP 没有拥塞控制，很适合多媒体通信的要求。
- UDP 支持一对一、一对多、多对一和多对多的交互通信。
- UDP 的首部开销小，只有 8 个字节。

在计算 UDP 的检测和时，利用了 12 字节伪首部的网络层的信息。



TCP协议特点：

- TCP 协议如何实现可靠传输
- TCP 协议如何实现流量控制
- TCP 协议如何避免网络拥塞



TCP 协议概述：TCP 是面向连接的传输层协议；每一条 TCP 连接只能有两个端点，每一条 TCP 连接只能是点对点的（一对一）；TCP 提供可靠交付的服务；TCP 提供全双工通信；面向字节流；

TCP 连接的端点不是主机，不是主机的 IP 地址，不是应用进程，也不是传输层的协议端口。**TCP 连接的端点叫做套接字（socket）。端口号拼接到 IP 地址即构成了套接字**。

TCP 连接 ::= {socket1, socket2} = {(IP1: port1), (IP2: port2)}



可靠传输的工作原理：停止等待协议（无差错情况；超时重传）

确认丢失和确认迟到（只要你没有告诉我你收到了，我就认为你没收到，我就要重传）

使用上述的确认和重传机制，我们就可以在不可靠的传输网络上实现可靠的通信。这种可靠传输协议常称为自动重传请求 ARQ（Automatic Repeat reQuest）。

停止等待协议的优点是简单，但是缺点是信道利用率太低。



流水线传输：发送方可连续发送多个分组，不必每发完一个分组就停顿下来等待对方的确认（ACK）。由于信道上一直有数据不间断地传送，这种传输方式可获得很高的信道利用率。

连续 ARQ 协议（发送窗口）

累计确认。



TCP报文段的首部格式（20 字节的固定首部 + 选择（长度可变 ）+ 填充）

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/GraspPackettoAnalysisTCPHeader.JPG" alt="GraspPackettoAnalysisTCPHeader" style="zoom:60%;" />

TCP 报文段的首部格式：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ComponentofTCPHeader.JPG" alt="ComponentofTCPHeader" style="zoom:50%;" />

URG 位：确定报文段是否是有较高的优先级。

ACK 位：如果是 0，确认号无效；是 1 时确认号才有效。

SYN 位：同步时要用到。

PSH 位：设置为 1 时在接受端的 TCP 缓存中优先给应用程序。

RST 位：若设置为 1 ，说明 TCP 会话出现了严重的错误，必须得释放连接。要重新建立连接才能正常通信。



TCP 中以字节为单位的滑动窗口技术（当发送窗口中的部分数据报文在传输过程中丢失时，接受端发送确认信息时使用 **SACK（选择性确认）**来告诉发送端部分丢失的数据报文中字节序列号范围）。



超时重传时间的选择：

TCP 每发送一个报文段，就对这个报文段设置一次计时器。只要计时器设置的重传时间到，但还没有收到确认，就要重传这一报文段。

新的 RTT_s = (1 - 𝛼) x (旧的 RTT_s) + 𝛼 x (新的 RTT 样本) （RFC2988 推荐的 𝛼 值为 1/8）

超时重传时间应略大于根据上述公式计算得出的加权平均往返时间 RTT_s 。



**TCP 的流量控制就是通过接收端不断告诉发送端当前的接收窗口有多大来实现的**。



拥塞控制的一般原理：

出现拥塞的条件：对资源需求的总和 > 可用资源

**拥塞控制**是一个全局性的过程，涉及到所有的主机、所有的路由器，以及降低网络传输性能有关的所有因素。

**流量控制**往往指在给定的发送端和接受端之间的点对点通信量的控制，它所要做的就是抑制发送端发送数据的速率，以便接收端来得及接收。

拥塞控制所起的作用：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/TheFunctionofCongestionControl.JPG" alt="TheFunctionofCongestionControl" style="zoom:55%;" />



慢开始和拥塞避免：

发送方维持拥塞窗口 cwnd (congestion window)

发送方控制拥塞窗口的原则是：

- 只要网络没有出现拥塞，拥塞窗口就再增大一些，以便把更多的分组发送出去。
- 只要网络出现拥塞，拥塞窗口就减小一些，以减少注入到网络中的分组数。



慢开始和拥塞避免算法的实现举例：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/SlowStartandAvoidCongestionAlgorithm.JPG" alt="SlowStartandAvoidCongestionAlgorithm" style="zoom:55%;" />



图中的窗口单位不使用字节而使用**报文段**。慢开始门限的初始值设置为 16 个报文段，即 ssthresh = 16 。

拥塞避免并非指完全能够避免了拥塞，利用以上的措施要完全避免网络拥塞还是不可能的。**拥塞避免是说在拥塞避免阶段把拥塞窗口控制为按线性规律增长，使网络比较不容易出现拥塞**。



快重传和快恢复：

快重传算法：首先要求接收方每收到一个失序的报文段后就立即发出重复确认。这样做可以让发送方及早知道有报文段没有到达接收方。

**当发送端收到连续三个重复的确认时，就执行「乘法减小」算法**，把慢开始门限 ssthresh 减半，但拥塞窗口 cwnd 现在不设置为 1，而是设置为慢开始门限 ssthresh 减半后的数值，然后开始执行拥塞避免算法（加法增大），使拥塞窗口缓慢地线性增大。（threshold）

从连续收到三个重复的确认转入拥塞避免：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ThreeRepeatAcknowledgementinTCPReno.JPG" alt="ThreeRepeatAcknowledgementinTCPReno" style="zoom:55%;" />



发送方的发送窗口的上限值应当取为 **接收方窗口** 和 **拥塞窗口** 这两个变量中较小的一个，即 `发送窗口的上限值 = Min[rwnd, cwnd]` 。



TCP 的传输连接管理：

传输连接有三个阶段，**即连接建立、数据传送和连接释放**。TCP 连接的建立都是采用客户服务器方式。

主动发起连接建立的应用进程叫做客户（client）；被动等待连接建立的应用进程叫做服务器（server）。



TCP 的连接建立：用三次握手建立 TCP 连接。

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ThreewayHandshake.JPG" alt="ThreewayHandshake" style="zoom:55%;" />

为什么需要三次握手：避免在一些情况下（服务器端再次收到同一客户端发来的、在网络中延误的 TCP 连接建立的请求）服务器端的计算机在发出一次 ACK = 1 的确认后，一直等待客户端的回复来进行通信时，资源白白的浪费（因为连接已先于建立）。



三次握手建立 TCP 连接时客户端和服务器端的状态变化：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ClientandServerStateDuringTCPConnectionEstablished.JPG" alt="ClientandServerStateDuringTCPConnectionEstablished" style="zoom:55%;" />



CLOSED、SYN_SENT、LISTEN、SYN_RCVD、ESTABLISHED 。



TCP 的连接释放：四次挥手

- 数据传输结束后，通信的双方都可释放连接。现在 A 的应用进程先向其 TCP 发出连接释放报文段，并停止再发送数据，主动关闭 TCP 连接。A 把连接释放报文段首部的 FIN = 1，其序列号 seq = u，等待 B 的确认。
- B 发出确认，确认号 ack = u + 1，而这个报文段自己的序号 seq = v 。TCP 服务器进程通知高层应用进程。从 A 到 B 这个方向的连接就释放了，TCP 连接处于半关闭状态。B 若发送数据，A 仍要接受。
- 若 B 已经没有要向 A 发送的数据，其应用进程就通知 TCP 释放连接。
- A 收到连接释放报文段后，必须发出确认。

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/ClientandServerStateDuringTCPConnectionReleased.JPG" alt="ClientandServerStateDuringTCPConnectionReleased" style="zoom:55%;" />



最后 TCP 连接必须经过时间 2MSL（2 个最长报文寿命）后才真正释放掉。



## 06 应用层

每一个应用层的协议就对应着计算机上的一个服务。



指引：

- 域名系统 DNS（Domain Name System）

- 动态主机配置协议 DHCP（Dynamic Host Configuration Protocol）

- 文件传送协议 FTP（File Transfer Protocol）

- 远程终端协议 TELNET

- 远程桌面 RDP（Remote Desktop Protocol）

- 超文本传输协议 HTTP（Hyper Text Transfer Protocol）

- 电子邮件（SMTP，POP3，IMAP）



DNS 服务作用：负责解析域名，将域名解析成 IP。

什么是域名：

根：.

顶级域名：com、edu、net、cn、org、gov

二级域名：91xueit、inhe

三级域名：dba  （一个例子：www.dba.91xueit.com）



域名解析测试： 

`ping www.91xueit.com` 

`nslookup www.91xueit.com`  



域名注册

域名解析的过程

需要安装自己的 DNS 服务器的几种场景：解析内网自己的域名；降低到 Internet 的域名解析流量；域环境。



DHCP 动态主机配置：静态 IP 地址；动态 IP 地址；

DHCP 客户端请求 IP 地址的过程



释放租约：`ipconfig /release` 

重新申请获得地址：`ipconfig renew` 



跨网段地址分配：需要在连接不同网段的路由器端口上执行命令 `Router(config-if)#ip helper-address 192.168.0.100` 

DHCP 客户端请求地址的过程其实就是其实就是 **逆向 ARP（根据 MAC 地址获取相应的 IP 地址，而 ARP 协议是已知 IP 地址来获得相应的 MAC 地址，都需要通过广播）** 



FTP协议：

FTP 使用的两个 TCP 链接：TCP 控制连接（控制进程）。标准端口为 21，用于发送 FTP 命令信息；TCP 数据连接（数据传送进程）。标准端口为 20，用于上传、下载数据。

主动模式：FTP 客户端告诉 FTP 服务端使用什么端口侦听；FTP 服务端使用端口号 20 和被 FTP 客户端告知的端口号建立连接 。

被动模式：服务端在指定范围内的某个端口被动等待客户端发起连接。

FTP 传输模式：文本模式（ASCII 模式，以文本序列传输数据）；二进制模式（Binary 模式，以二进制序列传输数据）

**在 FTP 服务端如果有防火墙，那么需要在防火墙开 20 和 21 端口，使用主动模式进行数据连接，若使用被动模式，则不能下载数据**。



telnet 使用 TCP 的 23 端口



RDP 协议

`net user administrator a1!` ：更改用户密码

`net user han a1! /add` ：添加用户

通过 telnet 命令远程连接后，在命令窗口中执行的一个例子中的几个命令：

`mkdir hanTest` 

`notepad.exe` 

`net user han a1! /add` 

`net localgroup administrators han /add` ：把用户 han 添加到管理员组

`shutdown -r -t 0` ：关机



将用户添加到远程桌面组 Remote Desktop Users 组（添加到 administrators 组的话权限太大了）

Server 是多用户操作系统，启用远程桌面可以多用户同时使用服务器。

XP 和 Windows 7 是单用户操作系统，不支持多用户同时登陆。



如何将本地硬盘映射到远程



WWW（World Wide Web）

URL 的一般形式是：`<协议>://<主机>:<端口>/<路径> `（由以冒号隔开的两大部分组成，并且在 URL 中的字符对大写或小写没有要求）

网站的标识：1）用不同端口；2）用不同的 IP 地址；3）使用主机头（域名）

安装 Web 服务，创建 Web 站点



http web

使用 Web 代理服务器访问网站的几种场景：

- 节省内网访问 Internet 的带宽
- 通过 Web 代理绕过防火墙



发送电子邮件：SMTP（简单邮件传输协议）。

接收电子邮件：POP3、IMAP。



1. 安装 POP3 和 SMTP 服务以及 DNS 服务
2. 在 DNS 服务器上创建 91xueit.com 和 51cto.com；创建主机记录 mail 192.168.80.100；创建邮件交换记录，MX 记录；
3. 在 POP3 服务上创建域名，创建邮箱
4. 配置 SMTP 服务器，创建远程域名 *.com，允许发送到远程
5. 配置 outlookExpress ，指明收件的服务器和发邮件的服务器；使用 POP3 协议收邮件



搭建能够在 Internet 上使用的邮件服务器

1. 在 Internet 上注册了域名 MX 记录
2. 邮件服务器有公网 IP 地址，或端口映射到邮件服务器  SMTP  TCP  25



## 07 网络安全

安全包括哪些方面：

- 数据存储安全
- 应用程序安全
- 操作系统安全
- 网络安全
- 物理安全
- 用户安全教育



指引：

- 网络安全问题概述

- 两类密码体制

- 数字签名

- 因特网使用的安全协议

- 链路加密与端到端加密

- 防火墙



计算机网络上的通信面临以下四种威胁：

1. 截获：从网络上窃听他人的通信内容
2. 中断：有意中断他人在网络上的通信
3. 篡改：故意篡改网络上传送的报文
4. 伪造：伪造信息在网络上传送

截获信息的攻击称为被动攻击；其余三种为主动攻击（拒绝用户使用资源或更改信息的攻击称为主动攻击）。



`arp -a` 

`ipconfig /flushdns` 



<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/FourTypesofNetworkAttack.JPG" alt="FourTypesofNetworkAttack" style="zoom:50%;" />



DoS (Denial of  Service) 攻击；DDoS 攻击（肉鸡）



恶意程序（rogue program）

1. 计算机病毒：会 “传染” 其他程序的程序，“传染” 是通过修改其他程序来把自身或其变种复制进去完成的。
2. 计算机蠕虫：通过网络的通信功能将自身从一个结点发送到另一个结点并启动运行的程序。（不断消耗计算机的资源，cpu、内存）
3. 特洛伊木马：一种程序，它执行的功能超出所声称的功能。（木马程序要和外界通讯，把获取的信息发送出去）
4. 逻辑炸弹：一种当运行环境满足某种特定条件时执行其他特殊功能的程序。



查木马程序的两种方法：

1. 查看会话 `netstat -n` 是否有可疑会话
2. 运行 msconfig 服务，隐藏微软服务来进行查看
3. 安装杀毒软件



加密技术

对称加密：加密秘钥和解密秘钥是同一个秘钥。

- 优点：效率高；缺点：秘钥不适合在网上传输，秘钥维护麻烦。
- 加密算法和加密秘钥。数据加密标准 DES。



非对称加密：加密秘钥和解密秘钥是不同的。密钥对：公钥和私钥。分两种情况，**公钥加密私钥解密和私钥加密公钥解密**。

非对称加密细节：中间利用了对称加密来提高加密的效率。

数字签名作用：防止抵赖，能够检查签名之后内容是否被更改。

证书颁发机构作用：为企业和用户颁发数字证书，确认这些企业和个人的身份；发布证书吊销列表；企业和个人信任证书颁发机构



WindowsServer2003  CA

WindowsXP1  zhang  zhang91xueit@yeah.net

WindowsXP2  wang  wang91xueit@yeah.net



Internet 上使用的安全协议

- 安全套接字 SSL（Secure Sockets Layer）
- 网络层安全 IPSec

链路加密与端到端加密

防火墙



SSL 的位置：

<img src="https://gitee.com/haokaimo/Picture/raw/master/ComputerNetworks/TheLocationofSSL.JPG" alt="TheLocationofSSL" style="zoom:50%;" />

在发送方，SSL 接收应用层的数据（如 HTTP 或 IMAP 报文），对数据进行加密，然后把加了密的数据送往 TCP 套接字。

在接收方，SSL 从 TCP 套接字读取数据，解密后把数据交给应用层。

http：TCP 80 端口；https：TCP 443 端口。

imaps：tcp 993 端口

pop3s：tcp 995 端口

smtps：tcp 465端口



SSL 提供以下三个功能：

1. SSL 服务器鉴别：允许用户证实服务器的身份。具有 SSL 功能的浏览器维持一个表，上面有一些可信赖的认证中心 CA（Certificate Authority）和它们的公钥。
2. 加密的 SSL 会话：客户和服务器交互的所有数据都在发送方加密，在接收方解密。
3. SSL 客户鉴别：允许服务器证实客户的身份。



网络层安全 IPSec

安全关联 SA（Security Association）

**在使用 AH 或 ESP 之前，先要从源主机到目的主机建立一条网络层的逻辑连接。此逻辑连接叫做安全关联 SA**。

IPSec 就把传统的因特网无法连接的网络层转换为具有逻辑连接的层。

SA （安全关联）是构成 IPSec 的基础，是两个通信实体经协商（利用 IKE 协议）建立起来的一种协定，它决定了用来保护数据分组安全的**安全协议（AH 协议或者 ESP 协议）、转码方式、密钥及密钥的有效存在时间等**。

IPSec 中最主要的协议：

- 鉴别首部 AH（Authentication）：AH 鉴别源点和检查数据完整性，但不能保密。（签名）
- 封装安全有效载荷 ESP（Encapsulation Security Payload）：ESP 比 AH 复杂得多，它鉴别源点、检查数据完整性和提供保密。（签名 + 加密）



数据链路层安全

数据链路层身份验证  PPP  身份验证

ADSL  数据链路层安全



防火墙（firewall）

防火墙是由软件、硬件构成的系统，是一种特殊编程的路由器，用来在两个网络之间实施接入控制策略。接入控制策略是由使用防火墙的单位自行制定，为的是可以最适合本单位的需要。

防火墙内的网络称为 “可信赖的网络”（trusted network），而将外部的因特网称为 “不可信赖的网络”（untrusted network）。

防火墙可用来解决内联网和外联网的安全问题。

防火墙技术一般分为两类：

1. 网络级防火墙：用来防止整个网络出现外来非法的入侵。属于这类的有**分组过滤和授权服务器**。前者检查所有流入本网络的信息，然后拒绝不符合事先制订好的一套准则的数据，而后者则是检查用户的登录是否合法。（网络层防火墙：基于数据包、源地址、目标地址、协议和端口，可以控制流量）
2. 应用级防火墙：从应用程序来进行接入控制。通常使用**应用网关或代理服务器来区分各种应用**。例如，可以只允许通过访问万维网的应用，而阻止 FTP 应用的通过。（应用层防火墙：基于数据包、源地址、目标地址、协议、端口、用户名、时间段、内容，可以防病毒进入内网）



防火墙网络拓扑结构：三像外围网；背靠背防火墙；单一网卡；边缘防火墙；



## 08 因特网上的音频视频

在 Internet 上传输音频视频面临哪些问题？

- 音频视频：占用的带宽高、网速恒定、延迟低。
- 数据信息：对带宽、网速是否恒定、延迟要求不高。



延迟：对于非交互式的音频视频影响不大

互联网中数据传输带宽不稳定



因特网提供的音频/视频服务大体上可分为三种类型：

1. 流式（streaming）存储音频/视频——边下载边播放（节省客户端硬盘空间，不用下载；保护视频版权）
2. 流式实况音频/视频——边录制边发送（通过网络现场直播）
3. 交互式音频/视频——实时交互式通信



 `mms://流媒体服务器地址/流媒体文件名` 



IP 电话概述：

狭义的 IP 电话：就是指在 IP 网络上打电话。所谓 “IP 网络” 就是 “使用 IP 协议的分组交换网” 的简称。

广义的 IP 电话：不仅仅是电话通信，而且还可以是在 IP 网络上进行交互式多媒体实时通信（包括语音、视像等），甚至包括即时传信 IM（Instant Messaging）。



改进「尽最大努力交付」的服务

服务质量 QoS 是服务性能的总效果，此效果决定了一个用户对服务的满意程度。

服务质量可用若干基本的性能指标来描述，包括可用性、差错率、响应时间、吞吐量、分组丢失率、连接建立时间、故障检测和改正时间等。服务提供者可向其用户保证某一种等级的服务质量。



## 09 无线网络

无线路由器可看成三合一的设备：路由器、交换机、无线接入点 AP。



&nbsp;

&nbsp;

> **版权声明：本文采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh) 许可协议，若转载请注明出处**。