---
title: openSUSE-源
date: '2014-09-12'
description:
categories:

tags:system

---

打开YaST中的 Software Repositories

选择Add，通过Specify URL方式，添加如下源：

	163-opensuse-11.4-update: http://mirrors.163.com/openSUSE/update/11.4/
	163-opensuse-11.4-oss: http://mirrors.163.com/openSUSE/distribution/11.4/repo/oss/
	163-opensuse-11.4-non-oss: http://mirrors.163.com/openSUSE/distribution/11.4/repo/non-oss/

其中Oss是指 Open Source Software，开源软件。那么Non-Oss就是非开源软件了。

另外Sohu也有openSUSE的源，但是在我用的网络，不如163的快。

	sohu-opensuse-11.4-update: http://mirrors.sohu.com/opensuse/update/11.4/
	sohu-opensuse-11.4-oss: http://mirrors.sohu.com/opensuse/distribution/11.4/repo/oss/
	sohu-opensuse-11.4-non-oss: http://mirrors.sohu.com/opensuse/distribution/11.4/repo/non-oss/

添加了国内的源，便可以不选择系统默认的官方SUSE源。更改Priority也可。系统就能飞速更新了。

---

	163-opensuse-13.1-update: http://mirrors.163.com/openSUSE/update/13.1/
	163-opensuse-13.1-oss: http://mirrors.163.com/openSUSE/distribution/13.1/repo/oss/
	163-opensuse-13.1-non-oss: http://mirrors.163.com/openSUSE/distribution/13.1/repo/non-oss/

	sohu-opensuse-13.1-update: http://mirrors.sohu.com/opensuse/update/13.1/
	sohu-opensuse-13.1-oss: http://mirrors.sohu.com/opensuse/distribution/13.1/repo/oss/
	sohu-opensuse-13.1-non-oss: http://mirrors.sohu.com/opensuse/distribution/13.1/repo/non-oss/


