---
title: Ubuntu下使用Hostapd搭建热点
date: '2017-07-17'
description:
categories:

tags:system

---

>

### Ubuntu下使用Hostapd搭建热点

>

	// 二选一(1)：准备工作: 安装 hostap 
	wget http://hostap.epitest.fi/releases/hostapd-2.2.tar.gz
	tar -zxf hostapd-2.2.tar.gz
	cd hostapd-2.2
	cd hostapd
	make

	// 二选一(2)：安装 hostap
	apt-get install hostapd

	// 打补丁
	git clone https://github.com/OpenSecurityResearch/hostapd-wpe
	patch -p1 < ../hostapd-wpe/hostapd-wpe.patch


>

	// /etc/hostapd.conf
	interface=wlan0
	driver=nl80211
	ssid=test
	channel=6
	hw_mode=g
	auth_algs=1
	wpa=3
	wpa_passphrase=12345678
	wpa_key_mgmt=WPA-PSK
	wpa_pairwise=TKIP
	rsn_pairwise=CCMP

>

	// 测试 
	hostapd /etc/hostapd.conf

>

---

>

	// 一般都会出现以下错误
	Configuration file: hostapd.conf
	nl80211: Could not configure driver mode
	nl80211 driver initialization failed.
	hostapd_free_hapd_data: Interface wlan0 wasn't started

>

	// 解决方案: 修改 NetworkManager 相关配置
	// 放弃NetworkManager对无线热点网卡的管理
	// /etc/NetworkManager/NetworkManager.conf
	[main]
	plugins=ifupdown,keyfile,ofono
	dns=dnsmasq

	[ifupdown]
	managed=false

	[keyfile] // 注意: 这里的MAC地址是 wlan0 的 .
	unmanaged-devices=mac:70:1c:e7:35:06:8d

>

	// 重启 NetworkManager
	sudo service network-manager stop;sudo service network-manager start  

>

---

>

*此时，应该已经可以搜索到ssid为test的wifi了，但是还不能连接*

>

	// 安装 dhcp server
	apt-get install isc-dhcp-server

>

	// 配置 DHCP
	// vi /etc/default/isc-dhcp-server
	INTERFACES="wlan0"

>

	option domain-name-servers 8.8.8.8, 114.114.114.114;
	default-lease-time 600;
	max-lease-time 7200;

	// 192.168.3.0 为网段
	subnet 192.168.3.0 netmask 255.255.255.0 {
		// range 为 dhcp 的地址池
		range 192.168.3.100 192.168.3.200;
		// routers 为 获取的网关 ...
		// 这里的地址为 wlan0 的 ip 地址 ...
		option routers 192.168.3.1;
		// 广播地址
		option broadcast-address 192.168.3.255;
	}

>

	// 配置 wlan0 地址并重启 dhcp server
	ifconfig wlan0 192.168.3.1/24
	service isc-dhcp-server restart

>

	// 开启端口转发 ... 并透明代理 eth0
	echo "1" > /proc/sys/net/ipv4/ip_forward
	iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

>

***完成以上配置，如果没有出错，应该可以了，此时需要写个脚本启动了***

>
