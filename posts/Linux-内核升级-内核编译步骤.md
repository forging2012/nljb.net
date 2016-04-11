---
title: Linux-内核升级-内核编译步骤
date: '2014-07-04'
description:
categories:

tags:system

---

	执行命令：
	#make clean
	然后是：
	#make mrproper
	进入图形配置界面。在终端敲入以下命令：
	#make menuconfig
	设置完毕，进入编译阶段。如果补丁和配置正确，下面几步不会出错，按顺序执行，等待完成即可。
	#make bzImage
	#make modules
	#make modules_install
	#make install

