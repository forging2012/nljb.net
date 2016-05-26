---
title: Linux之X11VNC通过VNC来连接服务器桌面
date: '2016-05-26'
description:
categories:

tags:system

---

>

### Linux之X11VNC通过VNC来连接服务器桌面

>

	x11vnc是一个可以让管理员直接通过VNC Viewer来连接服务器的桌面工具

	需要安装：x11vnc

	 | Name            | Summary                                 | Type
	 --+-----------------+-----------------------------------------+------------
	   | X11VNC          | Connect to remote X display through VNC | application
	   | X11VNC Server   | Share this desktop via VNC              | application
	 i | x11vnc          | VNC Server for Real X Displays          | package
	   | x11vnc-frontend | Simple GUI Frontend to x11vnc           | package


>

---

>

	// 启动脚本

	#!/bin/sh

	while true;
	do
		AUTH_ID=`ps -ef | grep auth | grep -v grep | grep -v xinitrc | awk '{print $11;}'`

		if [ "$AUTH_ID" = "" ]; then
			echo "Waiting for ready, retry 10s later..."
		else
			x11vnc -auth $AUTH_ID -display :0 -xdamage -xdamage -ncache_cr
		fi

		sleep 10
	done

>

---

>

	// 执行脚本后，在任意电脑安装VNC Viewer即可登录, 端口号一般从5900开始 ...

>
