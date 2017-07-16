---
title: 在Linux中使用lsusb查看USB设备
date: '2017-07-16'
description:
categories:

tags:system

---

>

### 在Linux中使用lsusb查看USB设备

>

	Bus 008 : 指明设备连接到哪（哪条总线）
	Device 002 : 表明这是连接到总线上的第二台设备
	ID : 设备的ID
	Broadcom Corp. Bluetooth Controller :生产商名字和设备名

>

	$ lsusb
	Bus 002 Device 002: ID 05e3:0612 Genesys Logic, Inc.
	Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
	Bus 001 Device 002: ID 05e3:0612 Genesys Logic, Inc.
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

	// Genesys 是台湾的 USB HUB 芯片厂商，我们可以看到在系统中同时使用了 USB 3.0 root hub 驱动和 USB 2.0 root hub 驱动。

>

---

>

	lsusb -v // 列出所有USB的详细

	// 可以输出非常详细的信息，包括设备的电流等等

>

---

>

	// 如果没有显示USB x.0 怎么办 Bus 001 Device 006: ID 80ee:0030 VirtualBox 
	Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
	Bus 001 Device 006: ID 80ee:0030 VirtualBox 
	Bus 001 Device 002: ID 80ee:0021 VirtualBox USB Tablet
	Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

>

	lsusb -t
	/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/6p, 5000M
	/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/8p, 480M
	    |__ Port 1: Dev 2, If 0, Class=Human Interface Device, Driver=usbhid, 12M
	    |__ Port 2: Dev 6, If 0, Class=Video, Driver=uvcvideo, 12M
	    |__ Port 2: Dev 6, If 1, Class=Video, Driver=uvcvideo, 12M


	lsusb -t
	/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/8p, 5000M
	/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/16p, 480M
	    |__ Port 8: Dev 2, If 1, Class=Wireless, Driver=btusb, 12M
	    |__ Port 8: Dev 2, If 0, Class=Wireless, Driver=btusb, 12M
	    |__ Port 9: Dev 3, If 0, Class=Video, Driver=uvcvideo, 480M
	    |__ Port 9: Dev 3, If 1, Class=Video, Driver=uvcvideo, 480M

>

	// 是指USB类型的传输速率
	12M 意味着 USB 1.0/1.1的速率是 12Mbit/s
	480M 意味着 USB 2.0的速率是 480Mbit/s
	5G 意味着 USB 3.0 的速率是 5Gbit/s

>

