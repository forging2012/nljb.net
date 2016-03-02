---
title: AutoIt的介绍与使用
date: '2016-03-02'
description:
categories:

tags:system

---

>

### AutoIt

>

<img src="{{urls.media}}/AutoIt的介绍与使用/1.png" alt="" width="800" height"500">

>

*AutoIt 目前最新是v3版本，这是一个使用类似BASIC脚本语言的免费软件,它设计用于Windows GUI(图形用户界面)中进行自动化操作*

*它利用模拟键盘按键，鼠标移动和窗口/控件的组合来实现自动化任务。*

*而这是其它语言不可能做到或无可靠方法实现的(例如VBScript和SendKeys).*

>

* 运行Windows和Dos程序
* 模拟键击动作(支持大多数键盘布局)
* 模拟鼠标移动和点击动作
* 对窗口进行移动,调整大小和其它操作
* 直接与窗口的“控件“交互(设置/获取文本,移动,关闭等等)
* 配合剪贴板进行剪切/粘贴文本操作
* 对注册表进行操作

>

	; 调用管理员权限	#RequireAdmin	; 运行	Run("q.exe")
	
	; 等待完成	WinWaitActive("[CLASS:#32770]", "立即安装")
	
	; 休眠	Sleep(500)
	
	; 点击	ControlClick("腾讯QQ安装向导","","[CLASS:Button; INSTANCE:3]")
	
	; 等待完成	WinWaitActive("[CLASS:#32770]", "完成安装")
	
	; 休眠	Sleep(500)
	
	；点击	ControlClick("腾讯QQ安装向导","","[CLASS:Button; INSTANCE:8]")
	
>
