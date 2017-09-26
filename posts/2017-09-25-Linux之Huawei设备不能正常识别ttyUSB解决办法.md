---
title: Linux之Huawei设备不能正常识别ttyUSB解决办法
date: '2017-09-25'
description:
categories:

tags:linux

---

>

### 解决 12d1:1446 -> 12d1:1436

>

***主要原因还是因为当前Huawei设备模式不对，一般如果处于存储模式就不会初始化ttyUSB设备等***

>

	sudo apt-get install usb-modeswitch

>

	DefaultVendor = 0x12d1
	DefaultProduct = 0x1446

	TargetVendor = 0x12d1
	TargetProductList = “1001,1406,140b，140C，1412,141b，14ac”

	CheckSuccess = 20

	在messageContent = “55534243123456780000000000000011062000000100000000000000000000”

>

	// 手动切换
	usb_modeswitch -c /etc/usb_modeswitch.d/12d1:1446

>

---

>

#### 解决 12d1:1f01 -> 12d1:14dc

>

	# /lib/udev/rules.d/40-usb_modeswitch.rules
	ATTR{idVendor}=="12d1", ATTR{idProduct}=="1f01", RUN+="usb_modeswitch '%b/%k'"
	ATTRS{idVendor}=="12d1", ATTRS{idProduct}=="1f01", RUN+="usb_modeswitch '%b/%k'"

>

	# /etc/usb_modeswitch.d/12d1\:1f01
	DefaultVendor=0x12d1
	DefaultProduct=0x1f01
	TargetVendor=0x12d1
	TargetProductList="14db,14dc"
	HuaweiNewMode=1
	NoDriverLoading=1

>

	# 测试
	usb_modeswitch -W -c /etc/12d1:1f01

>
