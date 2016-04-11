---
title: 对于WIFI密码中带有英文字符或特殊字符
date: '2014-07-04'
description:
categories:

tags:system

---

对于WIFI密码中带有英文字符或特殊字符需要使用S:

	1:iwlist eth1 scanning 查看无线路由 
	2:iwconfig eth1 essid "无线路由的名称" 
	3: ifconfig eth1 IP 
	4: route add default gw 网关 

对于带密码的路由器,设置如下: 

	1:iwconfig eth1 key s:密码 
	2:iwconfig eth1 key open 
	3:ifconfig eth1 essid "名称" 
	4:ifconfig eth1 IP5:route add default gw 网关

