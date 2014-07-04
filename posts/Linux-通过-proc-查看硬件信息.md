---
title: Linux-通过-proc-查看硬件信息
date: '2014-07-04'
description:
categories:

tags:system

---

用硬件检测程序kudzu探测新硬件：service kudzu start ( or restart)

	查看CPU信息：cat /proc/cpuinfo
	查看板卡信息：cat /proc/pci
	查看PCI信息：lspci (相比cat /proc/pci更直观）
	查看内存信息：cat /proc/meminfo
	查看USB设备：cat /proc/bus/usb/devices
	查看键盘和鼠标:cat /proc/bus/input/devices
	查看系统硬盘信息和使用情况：fdisk & disk - l & df
	查看各设备的中断请求(IRQ):cat /proc/interrupts
	查看系统体系结构：uname -a

在LINUX环境开发驱动程序，首先要探测到新硬件，接下来就是开发驱动程序。

常用命令整理如下：

	用硬件检测程序kudzu探测新硬件：service kudzu start ( or restart)
	查看CPU信息：cat /proc/cpuinfo
	查看板卡信息：cat /proc/pci
	查看PCI信息：lspci (相比cat /proc/pci更直观）
	查看内存信息：cat /proc/meminfo
	查看USB设备：cat /proc/bus/usb/devices
	查看键盘和鼠标:cat /proc/bus/input/devices
	查看系统硬盘信息和使用情况：fdisk & disk - l & df
	查看各设备的中断请求(IRQ):cat /proc/interrupts
	查看系统体系结构：uname –a
	 
看看系统认出的盘先： cat /proc/partitions

	dmidecode查看硬件信息，包括bios、cpu、内存等信息
	dmesg | more 查看硬件信息

对于“/proc”中文件可使用文件查看命令浏览其内容，文件中包含系统特定信息：

	Cpuinfo 主机CPU信息
	Dma 主机DMA通道信息
	Filesystems 文件系统信息
	Interrupts 主机中断信息
	Ioprots 主机I/O端口号信息
	Meninfo 主机内存信息
	Version Linux内存版本信息

