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

>

---

>

启动GDB后，首先就是要设置断点，程序中断后才能调试。

>

断点（BreakPoint）：

>

	在代码的指定位置中断，这个是我们用得最多的一种。设置断点的命令是break，它通常有如下方式：
	// 在进入指定函数时停住
	break <function> 
	// 在指定行号停住。
	break <linenum>
	// 在当前行号的前面或后面的offset行停住。offiset为自然数。
	break +/-offset
	// 在源文件filename的linenum行处停住。
	break filename:linenum 
	// ...可以是上述的参数，condition表示条件，在条件成立时停住。
	// 比如在循环境体中，可以设置break if i=100，表示当i为100时停住程序。
	break ... if <condition> 

	可以通过info breakpoints [n]命令查看当前断点信息。此外，还有如下几个配套的常用命令：
	// 删除所有断点
	delete
	// 删除某个断点
	delete breakpoint [n]
	// 禁用某个断点
	disable breakpoint [n] 
	// 使能某个断点
	enable breakpoint [n]

>

在gdb中，和调试步进相关的命令主要有如下几条：

>

	// 继续运行程序直到下一个断点（类似于VS里的F5）
	continue 
	// 逐过程步进，不会进入子函数（类似VS里的F10）
	next 
	// 逐语句步进，会进入子函数（类似VS里的F11）
	setp 
	// 运行至当前语句块结束
	until
	// 运行至函数结束并跳出，并打印函数的返回值（类似VS的Shift+F11）
	finish

 
