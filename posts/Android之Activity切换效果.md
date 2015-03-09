---
title: Android之Activity切换效果
date: '2015-03-09'
description:
categories:

tags:android

---

在Android开发过程中，经常会碰到Activity之间的切换效果的问题

下面介绍一下如何实现左右滑动的切换效果，首先了解一下Activity切换的实现

从Android2.0开始在Activity增加了一个方法：

>

	public void overridePendingTransition (int enterAnim, int exitAnim)

	// 其中：
	// enterAnim 定义Activity进入屏幕时的动画
	// exitAnim 定义Activity退出屏幕时的动画
	// overridePendingTransition 方法必须在startActivity()或者 finish()方法的后面。

	// 如果想关闭切换效果，只需要:
	overridePendingTransition(0, 0)
	// 还有个问题是，当用户点击返回键的时候
	// 系统会自动调用默认动画，返回到上一个Activity
	// 看了一些APP的解决办法是，拦截返回键事件，弹出窗口来处理
	// 窗口可以选择，是否需要退出，或者，是否需要返回上一级Activity

>




