---
title: Ubuntu之Firefox开机自启及全屏
date: '2017-08-22'
description:
categories:

tags:system

---

>

### Ubuntu之Firefox开机自启及全屏

>

	# 安装全屏插件 Firefox FullScreen
	https://addons.mozilla.org/zh-CN/firefox/addon/mwfullscreen/?src=search

	# 方法一：设置开机自动启动
	# vi ~/.config/upstart/firefox.conf
	start on desktop-start  
	stop on desktop-end  
	exec /usr/bin/firefox

	# 方法二：设置开机自动启动
	mkdir -p ~/.config/autostart
	cp /usr/share/applications/firefox.desktop ~/.config/autostart/firefox.desktop
	chmod +x ~/.config/autostart/firefox.desktop

	# 方法三：Startup Applications 添加

>

	# Chrome
	--kiosk 即可全屏
	--app=<URL> 自动打开指定的URL

>
