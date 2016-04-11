---
title: Linux-软件看门狗-watchdog
date: '2014-09-30'
description:
categories:

tags:system

---

Linux 自带了一个 watchdog 的实现，用于监视系统的运行

>

包括一个内核 watchdog module 和一个用户空间的 watchdog 程序。

>

内核 watchdog 模块通过 /dev/watchdog 这个字符设备与用户空间通信。

>

用户空间程序一旦打开 /dev/watchdog 设备（俗称“开门放狗”）

>

就会导致在内核中启动一个1分钟的定时器（系统默认时间）

>

此后，用户空间程序需要保证在1分钟之内向这个设备写入数据（俗称“定期喂狗”）

>

每次写操作会导致重新设定定时器。

>

如果用户空间程序在1分钟之内没有写操作，定时器到期会导致一次系统 reboot 操作（“狗咬人了”呵呵）。

>

通过这种机制，我们可以保证系统核心进程大部分时间都处于运行状态

>

即使特定情形下进程崩溃，因无法正常定时“喂狗”，Linux系统在看门狗作用下重新启动（reboot）

>

核心进程又运行起来了,多用于嵌入式系统。

---

打开 /dev/watchdog 设备（“开门放狗”）：

	int fd_watchdog = open("/dev/watchdog", O_WRONLY);  
	if(fd_watchdog == -1) {  
	    int err = errno;  
	    printf("\n!!! FAILED to open /dev/watchdog, errno: %d, %s\n", err, strerror(err));  
	    syslog(LOG_WARNING, "FAILED to open /dev/watchdog, errno: %d, %s", err, strerror(err));  
	}

---

每隔一段时间向 /dev/watchdog 设备写入数据（“定期喂狗”）： 

	//feed the watchdog  
	if(fd_watchdog >= 0) {  
	    static unsigned char food = 0;  
	    ssize_t eaten = write(fd_watchdog, &food, 1);  
	    if(eaten != 1) {  
		puts("\n!!! FAILED feeding watchdog");  
		syslog(LOG_WARNING, "FAILED feeding watchdog");  
	    }  
	} 

---

关闭 /dev/watchdog 设备，通常不需要这个步骤：

	close(fd_watchdog);    

---

所需头文件：

	#include <unistd.h>  
	#include <sys/stat.h>  
	#include <syslog.h>  
	#include <errno.h>  
