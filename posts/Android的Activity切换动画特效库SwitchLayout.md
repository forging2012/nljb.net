---
title: Android的Activity切换动画特效库SwitchLayout
date: '2015-06-25'
description:
categories:

tags:android

---

>

### SwitchLayout

>

	1.导入SwitchLayout1.0.jar或者下载开源库。

	2.每个Activity实现接口implements SwichLayoutInterFace。

	推荐这种用法；接口里分别实现2个方法：

		setEnterSwichLayout();
		setExitSwichLayout();
		// 这两个方法分别是设置进入Activity动画和离开Activity的动画的。

	在onCreate()里调用setEnterSwichLayout();

	在关闭Activity操作里调用setExitSwichLayout();

	如果需要的话在onKeyDown里拦截返回按键，调用setExitSwichLayout();

>

	SwitchLayout 的1.0jar包下载地址和Demo下载地址：http://pan.baidu.com/s/1dD6baLV

>

<img src="{{urls.media}}/Android的Activity切换动画特效库SwitchLayout/1.png" alt="" width="600">

>
