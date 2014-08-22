---
title: GDB
date: '2014-08-22'
description:
categories:

tags:system

---

GDB

---

	r(run)

要想运行准备调试的程序，可使用run命令，在它后面可以跟随发给该程序的任何参数.

如果你使用不带参数的run命令，gdb就再次使用你给予前一条run命令的参数，这是很有用的。

利用set args 命令就可以修改发送给程序的参数，而使用show args 命令就可以查看其缺省参数的列表。

>

	attach PID

众所周知，GDB有附着（attach）到正在运行的进程的功能，即attach <pid>命令。

因此我们可以利用该命令attach到子进程然后进行调试。

>

	gdb attach PID

同样可以命令行启动

>

	info threads 

显示当前可调试的所有线程。

每个线程会有一个GDB为其分配的ID，后面操作线程的时候会用到这个ID。

前面有*的是当前调试的线程。

>

	thread ID 

切换当前调试的线程为指定ID的线程。

>

	bt(backtrace)

列出调用栈
