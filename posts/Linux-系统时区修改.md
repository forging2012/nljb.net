---
title: Linux-系统时区修改
date: '2014-07-04'
description:
categories:

tags:system

---

	// 执行
	tzselect

	// 回车后会有选项提示
	TZ='Asia/Shanghai';export TZ
	// 即时生效 亚洲上海CST时间

	// 时区设置使重启后依然设生效
	hwclock -w

	// 时区配置查询文件
	/etc/sysconfig/clock

	// 时区配置文件：
	cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime 

	// 随后保存即可
	hwclock -w

