---
title: Linux之通过udev规则指定设备文件名
date: '2017-08-01'
description:
categories:

tags:system

---

>

### Linux之通过udev规则指定设备文件名

>

---

>

> 需求：有三个/dev/video0-3设备，但是每次重启设备的文件名称会改变, 想固定这个设备文件名
> 当前 /dev/video0 是 PCI 采集卡，1-2是USB摄像头 ...

>

	// 通过 udevinfo (ubuntu：udevadm) 查看设备信息
	// udevadm (udev管理工具)

>

	udevadm info -a /dev/video0 

		Udevadm info starts with the device specified by the devpath and then
		walks up the chain of parent devices. It prints for every device
		found, all possible attributes in the udev rules key format.
		A rule to match, can be composed by the attributes of the device
		and the attributes from one single parent device.

		  looking at device '/devices/pci0000:00/0000:00:1c.5/0000:02:00.0/0000:03:01.0/                                                                                                                                  0000:04:00.0/video4linux/video0':
		    KERNEL=="video0"
		    SUBSYSTEM=="video4linux"
		    DRIVER==""
		    ATTR{dev_debug}=="0"
		    ATTR{index}=="0"
		    ATTR{name}=="PL330B:RAW 00.00 a0011af2"

		  looking at parent device '/devices/pci0000:00/0000:00:1c.5/0000:02:00.0/0000:0                                                                                                                                  3:01.0/0000:04:00.0':
		    KERNELS=="0000:04:00.0"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="LINUXV4L2330b"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0xff0000"
		    ATTRS{consistent_dma_mask_bits}=="32"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0xa001"
		    ATTRS{dma_mask_bits}=="32"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="18"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0x00"
		    ATTRS{subsystem_device}=="0xa001"
		    ATTRS{subsystem_vendor}=="0x1af2"
		    ATTRS{vendor}=="0x1af2"

		  looking at parent device '/devices/pci0000:00/0000:00:1c.5/0000:02:00.0/0000:0                                                                                                                                  3:01.0':
		    KERNELS=="0000:03:01.0"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="pcieport"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0x060400"
		    ATTRS{consistent_dma_mask_bits}=="32"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0x2304"
		    ATTRS{dma_mask_bits}=="32"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="120"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0x05"
		    ATTRS{subsystem_device}=="0x0000"
		    ATTRS{subsystem_vendor}=="0x0000"
		    ATTRS{vendor}=="0x12d8"

		  looking at parent device '/devices/pci0000:00/0000:00:1c.5/0000:02:00.0':
		    KERNELS=="0000:02:00.0"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="pcieport"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0x060400"
		    ATTRS{consistent_dma_mask_bits}=="32"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0x2304"
		    ATTRS{dma_mask_bits}=="32"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="0"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0x05"
		    ATTRS{subsystem_device}=="0x0000"
		    ATTRS{subsystem_vendor}=="0x0000"
		    ATTRS{vendor}=="0x12d8"

		  looking at parent device '/devices/pci0000:00/0000:00:1c.5':
		    KERNELS=="0000:00:1c.5"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="pcieport"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0x060400"
		    ATTRS{consistent_dma_mask_bits}=="32"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0xa295"
		    ATTRS{dma_mask_bits}=="32"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="17"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0xf0"
		    ATTRS{subsystem_device}=="0x8694"
		    ATTRS{subsystem_vendor}=="0x1043"
		    ATTRS{vendor}=="0x8086"

		  looking at parent device '/devices/pci0000:00':
		    KERNELS=="pci0000:00"
		    SUBSYSTEMS==""
		    DRIVERS==""

>

	udevadm info -a /dev/video1 

		Udevadm info starts with the device specified by the devpath and then
		walks up the chain of parent devices. It prints for every device
		found, all possible attributes in the udev rules key format.
		A rule to match, can be composed by the attributes of the device
		and the attributes from one single parent device.

		  looking at device '/devices/pci0000:00/0000:00:14.0/usb1/1-5/1-5:1.0/video4linux/video1':
		    KERNEL=="video1"
		    SUBSYSTEM=="video4linux"
		    DRIVER==""
		    ATTR{dev_debug}=="0"
		    ATTR{index}=="0"
		    ATTR{name}=="USB 2.0 Camera"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1/1-5/1-5:1.0':
		    KERNELS=="1-5:1.0"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="uvcvideo"
		    ATTRS{authorized}=="1"
		    ATTRS{bAlternateSetting}==" 0"
		    ATTRS{bInterfaceClass}=="0e"
		    ATTRS{bInterfaceNumber}=="00"
		    ATTRS{bInterfaceProtocol}=="00"
		    ATTRS{bInterfaceSubClass}=="01"
		    ATTRS{bNumEndpoints}=="01"
		    ATTRS{iad_bFirstInterface}=="00"
		    ATTRS{iad_bFunctionClass}=="0e"
		    ATTRS{iad_bFunctionProtocol}=="00"
		    ATTRS{iad_bFunctionSubClass}=="03"
		    ATTRS{iad_bInterfaceCount}=="02"
		    ATTRS{interface}=="HD USB Camera"
		    ATTRS{supports_autosuspend}=="1"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1/1-5':
		    KERNELS=="1-5"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="usb"
		    ATTRS{authorized}=="1"
		    ATTRS{avoid_reset_quirk}=="0"
		    ATTRS{bConfigurationValue}=="1"
		    ATTRS{bDeviceClass}=="ef"
		    ATTRS{bDeviceProtocol}=="01"
		    ATTRS{bDeviceSubClass}=="02"
		    ATTRS{bMaxPacketSize0}=="64"
		    ATTRS{bMaxPower}=="500mA"
		    ATTRS{bNumConfigurations}=="1"
		    ATTRS{bNumInterfaces}==" 2"
		    ATTRS{bcdDevice}=="0100"
		    ATTRS{bmAttributes}=="80"
		    ATTRS{busnum}=="1"
		    ATTRS{configuration}==""
		    ATTRS{devnum}=="3"
		    ATTRS{devpath}=="5"
		    ATTRS{idProduct}=="9230"
		    ATTRS{idVendor}=="05a3"
		    ATTRS{ltm_capable}=="no"
		    ATTRS{manufacturer}=="HD Camera Manufacturer"
		    ATTRS{maxchild}=="0"
		    ATTRS{product}=="USB 2.0 Camera"
		    ATTRS{quirks}=="0x0"
		    ATTRS{removable}=="removable"
		    ATTRS{speed}=="480"
		    ATTRS{urbnum}=="7260181"
		    ATTRS{version}==" 2.00"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1':
		    KERNELS=="usb1"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="usb"
		    ATTRS{authorized}=="1"
		    ATTRS{authorized_default}=="1"
		    ATTRS{avoid_reset_quirk}=="0"
		    ATTRS{bConfigurationValue}=="1"
		    ATTRS{bDeviceClass}=="09"
		    ATTRS{bDeviceProtocol}=="01"
		    ATTRS{bDeviceSubClass}=="00"
		    ATTRS{bMaxPacketSize0}=="64"
		    ATTRS{bMaxPower}=="0mA"
		    ATTRS{bNumConfigurations}=="1"
		    ATTRS{bNumInterfaces}==" 1"
		    ATTRS{bcdDevice}=="0410"
		    ATTRS{bmAttributes}=="e0"
		    ATTRS{busnum}=="1"
		    ATTRS{configuration}==""
		    ATTRS{devnum}=="1"
		    ATTRS{devpath}=="0"
		    ATTRS{idProduct}=="0002"
		    ATTRS{idVendor}=="1d6b"
		    ATTRS{interface_authorized_default}=="1"
		    ATTRS{ltm_capable}=="no"
		    ATTRS{manufacturer}=="Linux 4.10.0-27-generic xhci-hcd"
		    ATTRS{maxchild}=="12"
		    ATTRS{product}=="xHCI Host Controller"
		    ATTRS{quirks}=="0x0"
		    ATTRS{removable}=="unknown"
		    ATTRS{serial}=="0000:00:14.0"
		    ATTRS{speed}=="480"
		    ATTRS{urbnum}=="60"
		    ATTRS{version}==" 2.00"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0':
		    KERNELS=="0000:00:14.0"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="xhci_hcd"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0x0c0330"
		    ATTRS{consistent_dma_mask_bits}=="64"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0xa2af"
		    ATTRS{dma_mask_bits}=="64"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="122"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0x00"
		    ATTRS{subsystem_device}=="0x8694"
		    ATTRS{subsystem_vendor}=="0x1043"
		    ATTRS{vendor}=="0x8086"

		  looking at parent device '/devices/pci0000:00':
		    KERNELS=="pci0000:00"
		    SUBSYSTEMS==""
		    DRIVERS==""

>

	udevadm info -a /dev/video2 

		Udevadm info starts with the device specified by the devpath and then
		walks up the chain of parent devices. It prints for every device
		found, all possible attributes in the udev rules key format.
		A rule to match, can be composed by the attributes of the device
		and the attributes from one single parent device.

		  looking at device '/devices/pci0000:00/0000:00:14.0/usb1/1-6/1-6:1.0/video4linux/video2':
		    KERNEL=="video2"
		    SUBSYSTEM=="video4linux"
		    DRIVER==""
		    ATTR{dev_debug}=="0"
		    ATTR{index}=="0"
		    ATTR{name}=="USB 2.0 Camera"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1/1-6/1-6:1.0':
		    KERNELS=="1-6:1.0"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="uvcvideo"
		    ATTRS{authorized}=="1"
		    ATTRS{bAlternateSetting}==" 0"
		    ATTRS{bInterfaceClass}=="0e"
		    ATTRS{bInterfaceNumber}=="00"
		    ATTRS{bInterfaceProtocol}=="00"
		    ATTRS{bInterfaceSubClass}=="01"
		    ATTRS{bNumEndpoints}=="01"
		    ATTRS{iad_bFirstInterface}=="00"
		    ATTRS{iad_bFunctionClass}=="0e"
		    ATTRS{iad_bFunctionProtocol}=="00"
		    ATTRS{iad_bFunctionSubClass}=="03"
		    ATTRS{iad_bInterfaceCount}=="02"
		    ATTRS{interface}=="HD USB Camera"
		    ATTRS{supports_autosuspend}=="1"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1/1-6':
		    KERNELS=="1-6"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="usb"
		    ATTRS{authorized}=="1"
		    ATTRS{avoid_reset_quirk}=="0"
		    ATTRS{bConfigurationValue}=="1"
		    ATTRS{bDeviceClass}=="ef"
		    ATTRS{bDeviceProtocol}=="01"
		    ATTRS{bDeviceSubClass}=="02"
		    ATTRS{bMaxPacketSize0}=="64"
		    ATTRS{bMaxPower}=="500mA"
		    ATTRS{bNumConfigurations}=="1"
		    ATTRS{bNumInterfaces}==" 2"
		    ATTRS{bcdDevice}=="0100"
		    ATTRS{bmAttributes}=="80"
		    ATTRS{busnum}=="1"
		    ATTRS{configuration}==""
		    ATTRS{devnum}=="4"
		    ATTRS{devpath}=="6"
		    ATTRS{idProduct}=="9230"
		    ATTRS{idVendor}=="05a3"
		    ATTRS{ltm_capable}=="no"
		    ATTRS{manufacturer}=="HD Camera Manufacturer"
		    ATTRS{maxchild}=="0"
		    ATTRS{product}=="USB 2.0 Camera"
		    ATTRS{quirks}=="0x0"
		    ATTRS{removable}=="removable"
		    ATTRS{speed}=="480"
		    ATTRS{urbnum}=="7275257"
		    ATTRS{version}==" 2.00"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0/usb1':
		    KERNELS=="usb1"
		    SUBSYSTEMS=="usb"
		    DRIVERS=="usb"
		    ATTRS{authorized}=="1"
		    ATTRS{authorized_default}=="1"
		    ATTRS{avoid_reset_quirk}=="0"
		    ATTRS{bConfigurationValue}=="1"
		    ATTRS{bDeviceClass}=="09"
		    ATTRS{bDeviceProtocol}=="01"
		    ATTRS{bDeviceSubClass}=="00"
		    ATTRS{bMaxPacketSize0}=="64"
		    ATTRS{bMaxPower}=="0mA"
		    ATTRS{bNumConfigurations}=="1"
		    ATTRS{bNumInterfaces}==" 1"
		    ATTRS{bcdDevice}=="0410"
		    ATTRS{bmAttributes}=="e0"
		    ATTRS{busnum}=="1"
		    ATTRS{configuration}==""
		    ATTRS{devnum}=="1"
		    ATTRS{devpath}=="0"
		    ATTRS{idProduct}=="0002"
		    ATTRS{idVendor}=="1d6b"
		    ATTRS{interface_authorized_default}=="1"
		    ATTRS{ltm_capable}=="no"
		    ATTRS{manufacturer}=="Linux 4.10.0-27-generic xhci-hcd"
		    ATTRS{maxchild}=="12"
		    ATTRS{product}=="xHCI Host Controller"
		    ATTRS{quirks}=="0x0"
		    ATTRS{removable}=="unknown"
		    ATTRS{serial}=="0000:00:14.0"
		    ATTRS{speed}=="480"
		    ATTRS{urbnum}=="60"
		    ATTRS{version}==" 2.00"

		  looking at parent device '/devices/pci0000:00/0000:00:14.0':
		    KERNELS=="0000:00:14.0"
		    SUBSYSTEMS=="pci"
		    DRIVERS=="xhci_hcd"
		    ATTRS{broken_parity_status}=="0"
		    ATTRS{class}=="0x0c0330"
		    ATTRS{consistent_dma_mask_bits}=="64"
		    ATTRS{d3cold_allowed}=="1"
		    ATTRS{device}=="0xa2af"
		    ATTRS{dma_mask_bits}=="64"
		    ATTRS{driver_override}=="(null)"
		    ATTRS{enable}=="1"
		    ATTRS{irq}=="122"
		    ATTRS{local_cpulist}=="0-7"
		    ATTRS{local_cpus}=="ff"
		    ATTRS{msi_bus}=="1"
		    ATTRS{numa_node}=="-1"
		    ATTRS{revision}=="0x00"
		    ATTRS{subsystem_device}=="0x8694"
		    ATTRS{subsystem_vendor}=="0x1043"
		    ATTRS{vendor}=="0x8086"

		  looking at parent device '/devices/pci0000:00':
		    KERNELS=="pci0000:00"
		    SUBSYSTEMS==""
		    DRIVERS==""
		
>

---

>

	// 查看设备对应的 KERNELS ...
	root@danoo-System-Product-Name:~# ll /sys/class/video4linux/video0/
	total 0
	drwxr-xr-x 3 root root    0 8月   1 22:16 ./
	drwxr-xr-x 3 root root    0 8月   1 13:58 ../
	-r--r--r-- 1 root root 4096 8月   1 13:58 dev
	-rw-r--r-- 1 root root 4096 8月   1 22:00 dev_debug
	lrwxrwxrwx 1 root root    0 8月   1 13:58 device -> ../../../0000:04:00.0/
	-r--r--r-- 1 root root 4096 8月   1 13:58 index
	-r--r--r-- 1 root root 4096 8月   1 22:00 name
	drwxr-xr-x 2 root root    0 8月   1 22:16 power/
	lrwxrwxrwx 1 root root    0 8月   1 13:58 subsystem -> ../../../../../../../../class/video4linux/
	-rw-r--r-- 1 root root 4096 8月   1 13:58 uevent
	root@danoo-System-Product-Name:~# ll /sys/class/video4linux/video1/
	total 0
	drwxr-xr-x 3 root root    0 8月   1 22:16 ./
	drwxr-xr-x 3 root root    0 8月   1 13:58 ../
	-r--r--r-- 1 root root 4096 8月   1 13:58 dev
	-rw-r--r-- 1 root root 4096 8月   1 22:02 dev_debug
	lrwxrwxrwx 1 root root    0 8月   1 13:58 device -> ../../../1-5:1.0/
	-r--r--r-- 1 root root 4096 8月   1 13:58 index
	-r--r--r-- 1 root root 4096 8月   1 22:02 name
	drwxr-xr-x 2 root root    0 8月   1 22:16 power/
	lrwxrwxrwx 1 root root    0 8月   1 13:58 subsystem -> ../../../../../../../../class/video4linux/
	-rw-r--r-- 1 root root 4096 8月   1 13:58 uevent
	root@danoo-System-Product-Name:~# ll /sys/class/video4linux/video2/
	total 0
	drwxr-xr-x 3 root root    0 8月   1 22:17 ./
	drwxr-xr-x 3 root root    0 8月   1 13:58 ../
	-r--r--r-- 1 root root 4096 8月   1 13:58 dev
	-rw-r--r-- 1 root root 4096 8月   1 22:03 dev_debug
	lrwxrwxrwx 1 root root    0 8月   1 13:58 device -> ../../../1-6:1.0/
	-r--r--r-- 1 root root 4096 8月   1 13:58 index
	-r--r--r-- 1 root root 4096 8月   1 22:03 name
	drwxr-xr-x 2 root root    0 8月   1 22:17 power/
	lrwxrwxrwx 1 root root    0 8月   1 13:58 subsystem -> ../../../../../../../../class/video4linux/
	-rw-r--r-- 1 root root 4096 8月   1 13:58 uevent

>

	// 在 /etc/udev/rules.d 下新建udev规则文件 81-uvccam.rules

	// video0 为 PCI 设备，通过 vendor 和 device 与 KERNELS 来指定 NAME 和 SYMLINK 名称
	// video0 的 device -> ../../../0000:04:00.0/ 同时用 0000:03:01.0 也可以 ... 只要包含 vendor 和 device 即可
	KERNEL=="video*", SUBSYSTEMS=="pci", ATTRS{vendor}=="0x1af2", ATTRS{device}=="0xa001", KERNELS=="0000:04:00.0",NAME="video0", SYMLINK+="x"
	# KERNEL=="video*", SUBSYSTEMS=="pci", ATTRS{vendor}=="0x12d8", ATTRS{device}=="0x2304", KERNELS=="0000:03:01.0",NAME="video0", SYMLINK+="x"
	// video1 为 USB 设备，通过 idVendor 和 idProduct 与 KERNELS 来指定 NAME 和 SYMLINK 名称
	// video1 的 device -> ../../../1-5:1.0/ 此时 KERNELS="1-5:1.0" 并不包含 idVendor 和 idProduct 但是 KERNELS="1-5" 包含同样可以使用 ...
	KERNEL=="video*", SUBSYSTEMS=="usb", ATTRS{idVendor}=="05a3", ATTRS{idProduct}=="9230", KERNELS=="1-5",NAME="video1", SYMLINK+="res"
	// video2 为 USB 设备，通过 idVendor 和 idProduct 与 KERNELS 来指定 NAME 和 SYMLINK 名称
	// video1 的 device -> ../../../1-6:1.0/ 此时 KERNELS="1-5:1.0" 并不包含 idVendor 和 idProduct 但是 KERNELS="1-6" 包含同样可以使用 ...
	KERNEL=="video*", SUBSYSTEMS=="usb", ATTRS{idVendor}=="05a3", ATTRS{idProduct}=="9230", KERNELS=="1-6",NAME="video2", SYMLINK+="face"

>

	// 重启后，即可看到

	lrwxrwxrwx 1 root root 6 8月   1 13:58 /dev/x -> video0
	lrwxrwxrwx 1 root root 6 8月   1 13:58 /dev/res -> video1
	lrwxrwxrwx 1 root root 6 8月   1 13:58 /dev/face -> video2

>

---

>

*声明：此方法通过 http://blog.csdn.net/linczone/article/details/48342419 学习, 内容绝对原创*

>
