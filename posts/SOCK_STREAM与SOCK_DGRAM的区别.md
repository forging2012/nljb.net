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

