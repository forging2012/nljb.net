---
title: Linux Integration Services
date: '2017-05-18'
description:
categories:

tags:system

---

>

#### Linux Integration Services

>

**ubuntu Integration Services for Hyper-V**

>

	// 通过管理员的方式打开
	sudo vi /etc/initramfs-tools/modules

	// 并在文件末尾添加如下指令：
	hv_vmbus
	hv_storvsc
	hv_blkvsc
	hv_netvsc

>


	// 更新：
	sudo update-initramfs –u

	// 关机：
	sudo shutdown -r now

>

	// 重新启动后输入：
	lsmod，就可以看到 hyper-v 被支持了

>
