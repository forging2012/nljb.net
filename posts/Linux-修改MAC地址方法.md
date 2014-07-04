---
title: Linux-修改MAC地址方法
date: '2014-07-04'
description:
categories:

tags:system

---

1.用命令行临时解决：

	#sudo ifconfig eth0 down
	#sudo ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF
	#sudo ifconfig eth0 up
	 
2.启动自行修改

	#sudo vi /etc/network/interfaces
	在eth0的配置中加入如下
	hwaddress ether AA:BB:CC:DD:EE:FF
	 
--------------------------------------------------------------------------------

方法一：

	1.关闭网卡设备
	ifconfig eth0 down

	2.修改MAC地址
	ifconfig eth0 hw ether MAC地址

	3.重启网卡
	ifconfig eth0 up
	 

方法二：

	在./etc/sysconfig/network-scripts/ifcfg-eth0中加入下面一句话：

	MACADDR=00:AA:BB:CC:DD:EE
	 
方法三：

	Linux 下如何更改网卡MAC地址 

	简单的办法是在/etc/rc.d/rc.sysinit文件中加入那些命令:

	ifconfig eth0 down
	ifconfig eth0 hw ether xx:xx:xx:xx:xx:xx
	ifconfig eth0 up

	因为这个脚本运行在network之前,所以,MAC跟IP就是对应的
 
方法四：

	Linux下的MAC地址更改,首先用命令关闭网卡设备。

	/sbin/ifconfig eth0 down

	然后就可以修改MAC地址了。

	/sbin/ifconfig eth0 hw ether xxxxxxxxxx （其中xx是您要修改的地址）

	最后重新启用网卡

	/sbin/ifconfig eth0 up

	网卡的MAC地址更改就完成了

