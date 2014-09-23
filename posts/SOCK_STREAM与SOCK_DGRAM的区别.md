---
title: SOCK_STREAM与SOCK_DGRAM的区别
date: '2014-09-23'
description:
categories:

tags:c++

---

SOCK_STREAM

是有保障的（即能保证数据正确传送到对方）面向连接的SOCKET，多用于资料（如文件）传送。 

SOCK_DGRAM

是无保障的面向消息的socket，主要用于在网络上发广播信息。

---

SOCK_STREAM是基于TCP的，数据传输比较有保障。

SOCK_DGRAM是基于UDP的，专门用于局域网，基于广播

SOCK_STREAM 是数据流,一般是tcp/ip协议的编程

SOCK_DGRAM分是数据抱,是udp协议网络编程

---

新套接口的类型描述类型，如TCP（SOCK_STREAM）和UDP（SOCK_DGRAM）。

常用的socket类型有，SOCK_STREAM、SOCK_DGRAM、SOCK_RAW、SOCK_PACKET、SOCK_SEQPACKET等等。

指定协议。套接口所用的协议。如调用者不想指定，可用0。

常用的协议有，IPPROTO_TCP、IPPROTO_UDP、IPPROTO_SCTP、IPPROTO_TIPC等

它们分别对应TCP传输协议、UDP传输协议、STCP传输协议、TIPC传输协议。

SOCK_STREAM 提供有序的、可靠的、双向的和基于连接的字节流，使用带外数据传送机制，为Internet地址族使用TCP。

SOCK_DGRAM 支持无连接的、不可靠的和使用固定大小（通常很小）缓冲区的数据报服务，为Internet地址族使用UDP。

SOCK_STREAM类型的套接口为全双向的字节流。

对于流类套接口，在接收或发送数据前必需处于已连接状态。

用connect()调用建立与另一套接口的连接

连接成功后，即可用send()和recv()传送数据。当会话结束后，调用closesocket()。

带外数据根据规定用send()和recv()来接收。

实现SOCK_STREAM类型套接口的通讯协议保证数据不会丢失也不会重复。

如果终端协议有缓冲区空间，且数据不能在一定时间成功发送，则认为连接中断，其后续的调用也将以WSAETIMEOUT错误返回。

SOCK_DGRAM类型套接口允许使用sendto()和recvfrom()从任意端口发送或接收数据报。

如果这样一个套接口用connect()与一个指定端口连接，则可用send()和recv()与该端口进行数据报的发送与接收。
