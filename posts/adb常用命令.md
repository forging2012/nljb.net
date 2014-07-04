---
title: adb常用命令
date: '2014-07-03'
description:
categories:

tags:android

---

	adb devices //列出所有的连接设备
	adb connect <host>[:<port>] //通过tcp/ip连接，5555是默认端口
	设备命令：
	adb push <local> <remote>  //拷贝文件/目录到设备
	adb pull <remote> [<local>] //从设备拷贝文件/目录
	adb sync [<directory>] //只有发生改变时从主机拷贝到设备
	adb shell  //运行远端shell交互
	adb shell <command> //运行远端shell 命令
	adb emu <command> //运行仿真控制台命令
	adb logcat [<filter-spec>] //浏览设备日志
	adb forward <local> <remote> //转发套接字连接
	adb install [-l] [-r] [-s] <file> //拷贝文件包到设备并安装
	adb uninstall [-k] <package> //卸载程序包，-k意味着保留数据和缓存
	adb bugreport //返回所有的bugreport信息
	adb help
	adb version
	脚本：
	adb wait-for-device //阻塞直到设备上线
	adb start-server
	adb kill-server
	adb get-state //列印offline|bootloader|device信息
	adb get-serialno
	adb status-window //连续列印设备状态
	adb remount //重装载/system分区
	adb reboot [bootloader|recomry]
	adb reboot-bootloader
	adb root
	adb usb


