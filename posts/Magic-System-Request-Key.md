---
title: Magic System Request Key
date: '2015-01-13'
description:
categories:

tags:system

---


在使用任何办法都无法重启系统时，可以尝试下面的方法

>

此方法放弃保存当前运行的任何数据，直接重启，但是却对硬盘无害。

>

	echo 1 > /proc/sys/kernel/sysrq
	echo b > /proc/sysrq-trigger

---

>

### /proc/sys/kernel/sysrq

>

向sysrq文件中写入1是为了开启SysRq功能。

>

根据linux/Documentations/sysrq.txt中所说：SysRq代表的是Magic System Request Key。

>

开启了这个功能以后，只要内核没有挂掉，它就会响应你要求的任何操作。

>

但是这需要内核支持(CONFIG_MAGIC_SYSRQ选项)。

>

向/proc/sys/kernel/sysrq中写入0是关闭sysrq功能，写入1是开启，其他选项请参考sysrq.txt。

>

需要注意的是，/proc/sys/kernel/sysrq中的值只影响键盘的操作。

>

	// 将会立即重启系统，并且不会管你有没有数据没有写回磁盘，也不卸载磁盘，而是完完全全的立即重启
	'b'
	// 将会关机
	'o'
	// 将会同步所有以挂在的文件系统
	's'
	// 将会重新将所有的文件系统挂在为只读属性
	'u'

---

### /proc/sysrq-trigger

>

从文件名字就可以看出来这两个是有关系的。

>

写入/proc/sysrq-trigger中的字符其实就是sysrq.txt中说的<command key>键所对应的字符，其功能也和上述一样。

>

所以，这两行命令先开启SysRq功能，然后用'b'命令让计算机立刻重启。
