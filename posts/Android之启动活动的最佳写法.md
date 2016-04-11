---
title: Android之启动活动的最佳写法
date: '2015-05-04'
description:
categories:

tags:android

---

>

### 启动活动的最佳写法

>

启动活动的方法常用的无非两种:

	startActivity()
	startActivityForResult()

>

如果需要传输数据无非使用:

	Intent intent = new Intent(this, ClassName.class);
	intent.putExtra("param1", "data1");
	...

>

---

>

### 问题随之产生

>

如果需要启动另外一个活动，则需要知道启动时所需传入的参数或数据

	一，去看要启动活动的代码
	二，去问实现该活动的码农

>

---

>

### 更好的办法

>

在你实现活动的同时，实现一个静态方法

	public static void actionStart(Context context, String data1, ...) {
		Intent intent = new Intent(context, ClassName.class);
		intent.putExtra("param1", "data1");
		...
		context.startActivity(intent)
		// 在原函数监听返回
		// context.startActivityForResult()
	}

>

这样，需要启动一个活动的时候，调用该类的actionStart即可，而且知道了需要的参数

	ClassName.actionStart(this, data1, ...)


