---
title: IOS之应用的生命周期
date: '2016-02-29'
description:
categories:

tags:ios

---

>

### 应用的生命周期

>

*AppDelegate类在应用声明周期的不同阶段会回调不同的方法*

>

<img src="{{urls.media}}/IOS之应用的生命周期/1.png" alt="" width="400">

>

* Not running		未运行
* Inactive		未激活        
* Active			激活           
* Backgroud		后台          
* Suspended		挂起

>

* 未运行:	程序没启动
* 未激活:	程序在前台运行，不过没有接收到事件。在没有事件处理情况下程序通常停留在这个状态
* 激活:		程序在前台运行而且接收到了事件。这也是前台的一个正常的模式
* 后台:		程序在后台而且能执行代码，大多数程序进入这个状态后会在在这个状态上停留一会
* ... 时间到之后会进入挂起状态(Suspended)。有的程序经过特殊的请求后可以长期处于Backgroud状态
* 挂起: 	程序在后台不能执行代码。系统会自动把程序变成这个状态而且不会发出通知
* ... 当挂起时，程序还是停留在内存中，当内存低时，就把挂起的程序清除掉，为前台提供更多的内存。

>

	// 	这是程序启动时调用的函数
	// 可以在此方法中加入初始化相关的代码
	application:didFinishLaunchingWithOptions   
	
	// 应用在准备进入前台运行时执行的函数
	// 当应用从启动到前台，或从后台转入前台都会调用此方法
	applicationDidBecomeActive
	
	// 应用当前正要从前台运行状态离开时执行的函数
	applicationWillResignActive
	
	// 此时应用处在background状态，并且没有执行任何代码
	// 未来将被挂起进入suspended状态。
	applicationDidEnterBackground 
	
	// 当前应用正从后台移入前台运行状态
	// 但是当前还没有到Active状态时执行的函数。
	applicationWillEnterForeground
	
	// 当前应用即将被终止，在终止前调用的函数
	// 如果应用当前处在suspended，此方法不会被调用
	applicationWillTerminate
	
>