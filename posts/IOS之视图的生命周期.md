---
title: IOS之视图的生命周期
date: '2016-03-01'
description:
categories:

tags:ios

---

>

### 视图的生命周期

>

*在视图不同的生命周期中，视图控制器会回掉不同的方法*

>

<img src="{{urls.media}}/IOS之视图的生命周期/1.jpg" alt="" width="600">

>

* 在视图控制器已被实例化，视图被加载到内存中，会调用viewDidLoad方法
* ... 这时视图并未出现，在该方法中通常会对所有控制的视图进行初始化处理
* ... viewDidLoad方法在应用的时候只调用一次 ...

>

* 视图可见前后会调用viewWillAppear方法和viewDidAppear方法
* 视图不可见前后会调用viewWillDisappear方法和viewDidDisappear方法
* ... 上述这四个方法可以被反复调用多次 ...

>

*在低内存情况下，IOS会调用didReceiveMemoryWarning方法，主要是释放内存，包括视图控制器中的一些成员变量*

>

---
