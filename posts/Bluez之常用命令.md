---
title: Bluez之常用命令
date: '2014-12-26'
description:
categories:

tags:system

---

启动服务：

	 /usr/lib/bluetooth/bluetoothd -C (兼容性)

配置文件：

	/usr/lib/systemd/system/bluetooth.service

	/etc/bluetooth/ (hcid.conf,main.conf,rfcomm.conf)

安装软件：

	bluez

	obexd

	sobexsrv (接收)

命令：

	hciconfig hci0 up

	hcitool scan 

	hcidump (状态)

	l2ping (Ping)

	hciconfig hci0 iscan (可见)

	hciconfig hci0 pscan (开启Inquiry Scan和Page Scan,使手机处于可被搜索和可连接状态)

服务：

	service bluetooth start

	bluetoothctl (工具)

本地增加所有服务：

  	sdptool add --channel=1 DID SP DUN LAN FAX OPUSH FTP HS HF SAP NAP GN PANU HID CIP CTP A2SRC A2SNK SYNCML NOKID PCSUITE SR1

检查手机：

	sdptool browse 00:66:4B:40:91:44

联接手机：

	rfcomm bind 0 00:66:4B:40:91:44 1 (使用的接口及频道)

	cat >/dev/rfcomm0 (配对)

发送：

	obex_test -b 00:66:4B:40:91:44 7 (需要选择模式和信道)

	obexftp -b 00:66:4B:40:91:44 -B 12 -U none -p /root/json.json

	obexfs …



