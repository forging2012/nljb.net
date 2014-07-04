---
title: Linux-下添加路由的方法
date: '2014-07-04'
description:
categories:

tags:system

---

一,使用route 命令添加

	使用route 命令添加的路由，机器重启或者网卡重启后路由就失效了，方法：

	//添加到主机的路由
	# route add –host 192.168.168.110 dev eth0
	# route add –host 192.168.168.119 gw 192.168.168.1
	//添加到网络的路由
	# route add –net IP netmask MASK eth0
	# route add –net IP netmask MASK gw IP
	# route add –net IP/24 eth1
	//添加默认网关
	# route add default gw IP
	//删除路由
	# route del –host 192.168.168.110 dev eth0

二,在linux下设置永久路由的方法：

	1.在/etc/rc.local里添加
	方法：
	route add -net 192.168.3.0/24 dev eth0
	route add -net 192.168.2.0/24 gw 192.168.3.254

	2.在/etc/sysconfig/network里添加到末尾
	方法：GATEWAY=gw-ip 或者GATEWAY=gw-dev

	3./etc/sysconfig/static-router :
	any net x.x.x.x/24 gw y.y.y.y 

