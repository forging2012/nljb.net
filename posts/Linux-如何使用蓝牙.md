---
title: Linux-如何使用蓝牙
date: '2014-12-10'
description:
categories:

tags:system

---

lsusb

>

<img src="{{urls.media}}/Linux-如何使用蓝牙/1.png" alt="" width="600">

---

运行hciconfig可以看到：

>

<img src="{{urls.media}}/Linux-如何使用蓝牙/2.png" alt="" width="600">

>

从上图可以看出，我们的蓝牙设备是 hci0

>

查看我们的蓝牙设备的硬件地址:

	hcitool dev

>

运行hcitoo --help 可以查看更多相关命令

---

然后我们激活它：

	sudo hciconfig hci0 up

要注意的是，激活前蓝牙必须是打开的，否则会出现如下错误：

>

<img src="{{urls.media}}/Linux-如何使用蓝牙/3.png" alt="" width="600">

---

然后我们开始扫描了：

	hcitool scan

<img src="{{urls.media}}/Linux-如何使用蓝牙/4.png" alt="" width="600">

>

可以看到，发现了我手机的蓝牙了~~

>

然后我们要开始连接了，连接阶段使用的主要命令是

	rfcomm

>

查看用法:

	rfcomm --help

>

首先需要绑定目的蓝牙设备：

	sudo rfcomm bind /dev/rfcomm0 E0:A6:70:8C:A3:02

>

注意：上面的这个地址是目的蓝牙设备的硬件地址

>

接着我们连接它：

	sudo cat >/dev/rfcomm0

>

这是目的蓝牙主机就会弹出一个对话框要求输入pin码，随便输入一个.

然后主机就会弹出一个对话框，只要输入的和刚才一致就可以通过验证。

之后我们发现我的手机已经显示了成功配对的标记了。

---

在配对完成之后我们需要删除绑定（否则在下次使用时会提示设备正忙），命令如下：

	sudo rfcomm release /dev/rfcomm0

