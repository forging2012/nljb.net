---
title: openSUSE之Broadwell-U的Graphics驱动方法
date: '2015-07-23'
description:
categories:

tags:system

---

>

VGA -> Intel Corporation Broadwell-U Integrated Graphics (https://01.org/zh/linuxgraphics)

>

	支持列表的核显型号有Broadwell-U处理器的HD Graphics 5500/6000/6100

	Intel Graphics 驱动支持从 2014Q2 开始支持 Broadwell-U 处理器

	Intel Graphics 驱动 2014Q2 驱动安装内核要求 Linux Kernel - 3.15 +

	Intel Graphics 驱动最新 2015Q1 兼容内核 Linux Kernel - 3.19.2 

	Intel Graphics 2015Q1 兼容显卡与 2014Q2 无区别，主要是修复BUG

	openSUSE 13.1 -> libva info: va_getDriverName() returns -1  (3.11.6-4-desktop) (内核不匹配)
	 
	openSUSE 13.2 -> 内核 (3.16.6-2-desktop) 匹配 Intel Graphics for Linux* - 2014Q3 Intel Graphics Stack Release

	源码安装一下驱动相关软件时所需配套软件几乎都可以在YAST中找到 ...

>

---

>

安装 libva -> Libva - 1.4.0 升级到 Libva - 1.5.1 (./configure --prefix=/usr)

>

<img src="{{urls.media}}/openSUSE之Broadwell-U的Graphics驱动方法/1.png" alt="" width="600" hight="200" >

>

---

>

安装 -> xf86-video-intel - 2.99.911 升级到 xf86-video-intel - 2.99.917

>

<img src="{{urls.media}}/openSUSE之Broadwell-U的Graphics驱动方法/2.png" alt="" width="600" hight="120" >

>

---

>

安装 -> vaapi intel-driver - 1.4.0 升级到 vaapi intel-driver - 1.5.1

>

<img src="{{urls.media}}/openSUSE之Broadwell-U的Graphics驱动方法/3.png" alt="" width="600" hight="600" >



