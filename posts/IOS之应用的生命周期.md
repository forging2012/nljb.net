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

***非运行状态-应用启动场景***

*用户点击图标，可能是第一次启动这个应用，也可能是应用终止后再次启动*

	状态：Not running（未运行）-> Inactive(未激活） -> Active（激活）
	阶段：Not running（未运行）-> Inactive(未激活）= application:didFinishLaunchingWithOptions
	阶段：Inactive(未激活）-> Active（激活） = applicationDidBecomeActive

>

***点击Home键-应用退出场景***

*应用处于运行状态(Active)时点击Home键或者其它应用程序导致当前应用中断*

*该场景的跃迁状态可以分成两种情况：可以在后台运行或挂起，不可以在后台运行或挂起, 取决于产品属性的配置*

*配置属性：Application does not run in background*

>

***可以在后台运行或挂起***

	状态：Active（激活）-> Inactive(未激活） -> Backgroud(后台) -> Suspended(挂起）
	阶段：Active（激活）-> Inactive(未激活）= applicationWillResignActive
	阶段：Inactive(未激活） -> Backgroud(后台) ＝ （ 不涉及说明的方法和通知 ）
	阶段：Backgroud(后台) -> Suspended(挂起） ＝ applicationDidEnterBackground
	
>
	
***不可以在后台运行或挂起***

	状态：Active（激活）-> Inactive(未激活） -> Backgroud(后台) -> Suspended(挂起）-> Not running
	阶段：Active（激活）-> Inactive(未激活）= （ 不涉及说明的方法和通知 ）
	阶段：Inactive(未激活） -> Backgroud(后台) = （ 不涉及说明的方法和通知 ）
	阶段：Backgroud(后台) -> Suspended(挂起） ＝ applicationDidEnterBackground
	阶段：Suspended(挂起）-> Not running = applicationWillTerminate
	
>

***挂起重新运行场景***

*挂起状态下的应用重新运行，所经历的场景与调用的方法*

	状态：Suspended(挂起）-> Backgroud(后台) -> Inactive(未激活） -> Active（激活)
	阶段：Suspended(挂起）-> Backgroud(后台) = （ 不涉及说明的方法和通知 ）
	阶段：Backgroud(后台) -> Inactive(未激活）＝ applicationWillEnterForeground
	阶段：Inactive(未激活） -> Active（激活) ＝ applicationDidBecomeActive
	
>

***内存清除-应用终止场景***

*应用在后台处理完成时进入挂起状态（这是一种休眠状态），发生低内存时，为了满足其它应用，该应用会被终止*

	状态：Backgroud(后台) -> Suspended(挂起）-> Not running
	阶段：该内存清除场景下，应用不会调用任何方法，也不会发出任何通知
	
>

---


