---
title: 笔记-计算机网络基础-TCP/IP vs OSI
categories:
  - 项目管理的培训笔记
  - 计算机网络基础
tags:
  - OSI
  - TCP/IP
  - 计算机网络基础
abbrlink: cf6f
date: 2020-04-29 23:10:47
updated: 2020-04-29 23:10:47
---

## 1. TCP/IP vs OSI

![TCP/IP vs OSI](https://i.loli.net/2020/04/29/pLwTC2z3i6cQld5.png)

- 物理层具体标准有：RS232、V.35、RJ-45、FDDI
- 数据链路层常用协议有：IEEE802.3/.2、HDLC、PPP、ATM
- 网络层的具体协议有：IP、ICMP、IGMP、IPX、ARP等
- 会话层常见协议有：RPC 、SQL 、NFS
- 表示层常见协议有：JPEG 、ASCII 、DES 、MPEG
- 应用层常见协议有：HTTP、Tenlet、FTP、SMTP等
<!-- more -->

## 2. TCP/IP主要协议

### 2.1. 1)网络层协议

**IP(Internet Protocol）协议**
`IP`是网络层的核心，通过路由选择将下一条IP封装后交给接口层。IP数据报是无连接服务。

**Ping**
`Ping`命令就是发送ICMP的echo包，通过回送的echo relay进行网络测试。
  
**ICMP(Internet Control Message Protocol)控制报文协议**
`ICMP`是网络层的补充，可以回送报文。用来检测网络是否通畅。

Internet控制报文协议，它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。控制消息是指网络通不通、主机是否可达、路由是否可用等网络本身的消息。ICMP协议是一种面向无连接的协议，用于传输出错报告控制信息。

**ARP(Address Resolution Protocol）地址转换协议**
`ARP`是正向地址解析协议，通过已知的IP，寻找对应主机的MAC地址。

**RARP(Reverse ARP)反向地址转换协议**
`RARP`是反向地址解析协议，通过MAC地址确定IP地址。比如无盘工作站还有DHCP服务。

**IGMP**
IGMP协议（Internet Group Management Protocol）Internet 组管理协议称为，是因特网协议家族中的一个组播协议。该协议运行在主机和组播路由器之间。

### 2.2. 2)传输层协议

**传输控制协议TCP(Transmission Control Protocol）**
TCP是面向连接的通信协议，通过三次握手建立连接，通讯完成时要拆除连接，由于TCP是面向连接的所以只能用于点对点的通讯。

TCP提供的是一种可靠的数据流服务，采用“带重传的肯定确认”技术来实现传输的可靠性。

**用户数据报协议UDP(User Datagram protocol）**
UDP是面向无连接的通讯协议，UDP数据包括目的端口号和源端口号信息，由于通讯不需要连接，所以可以实现广播发送。

UDP通讯时不需要接收方确认，属于不可靠的传输，可能会出丢包现象，实际应用中要求程序员编程验证。UDP与TCP位于同一层，但它不管数据包的顺序、错误或重发。因此，UDP不被应用于那些使用虚电路的面向连接的服务，UDP主要用于那些面向查询---应答的服务，例如NFS。相对于FTP或Telnet，这些服务需要交换的信息量较小。使用UDP的服务包括NTP（网络时间协议）和DNS（DNS也使用TCP）。

### 2.3. 3)应用层协议

**FTP(File Transfer Protocol）**
是文件传输协议，一般上传下载用FTP服务，数据端口是20H，控制端口是21H。**TCP**

**Telnet**
服务是用户远程登录服务，使用23端口，使用明码传送，保密性差、简单方便。**TCP**

Telnet（远程登录协议）是登录和仿真程序，建立在TCP 之上，它的基本功能是允许用户登录进入远程计算机系统。以前，Telnet 是一个将所有用户输入送到远程计算机进行处理的简单的终端程序。目前，它的一些较新的版本是在本地执行更多的处理，可以提供更好的响应，并且减少了通过链路发送到远程计算机的信息数量。

**SMTP(Simple Mail Transfer Protocol）**
是简单邮件传输协议，用来控制信件的发送、中转，使用端口25。**TCP**

SMTP(Simple Mail Transfer Protocol）即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。SMTP协议属于TCP/IP协议簇，它帮助每台计算机在发送或中转信件时找到下一个目的地。通过SMTP协议所指定的服务器,就可以把E-mail寄到收信人的服务器上了，整个过程只要几分钟。SMTP服务器则是遵循SMTP协议的发送邮件服务器，用来发送或中转发出的电子邮件。

**NFS（Network File System）**
是网络文件系统，用于网络中不同主机间的文件共享。

**HTTP(Hypertext Transfer Protocol）**
是超文本传输协议，用于实现互联网中的WWW服务，使用端口80。

**TFTP（Trivial File Transfer Protocol,简单文件传输协议）**
是TCP/IP协议族中的一个用来在客户机与服务器之间进行简单文件传输的协议，提供不复杂、开销不大的文件传输服务。端口号为69。**基于UDP**。

**DNS(Domain Name Service）**
是域名解析服务，提供域名到IP地址之间的转换，使用端口53。**基于UDP**。

**DHCP**
DHCP

转载/整理：
希赛教育的试题解释：<https://www.educity.cn/>
