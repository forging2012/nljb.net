---
title: Android之PopupWindow和AlertDialog区别
date: '2015-03-06'
description:
categories:

tags:android

---

本质区别为：

>

AlertDialog是非阻塞式对话框：AlertDialog弹出时，后台还可以做事情；

而PopupWindow是阻塞式对话框：PopupWindow弹出时，程序会等待

在PopupWindow退出前，程序一直等待，只有当我们调用了dismiss方法的后，PopupWindow退出，程序才会向下执行。

>

这两种区别的表现是：AlertDialog弹出时，背景是黑色的

但是当我们点击背景，AlertDialog会消失，证明程序不仅响应AlertDialog的操作，还响应其他操作

其他程序没有被阻塞，这说明了AlertDialog是非阻塞式对话框；

PopupWindow弹出时，背景没有什么变化

但是当我们点击背景的时候，程序没有响应，只允许我们操作PopupWindow，其他操作被阻塞。

>

	PopupWindow pw = new PopupWindow(view,width,height);
	// 重新设置PopupWindow的内容
	pw.setContentView(popupconten);
	// 默认是false，为false时，PopupWindow没有获得焦点能力
	// 如果这是PopupWindow的内容中有EidtText，需要输入，这是是无法输入的；
	// 只有为true的时候，PopupWindow才具有获得焦点能力，EditText才是>真正的EditText。
	pw.setFocusable(true);
	// 设置PopupWindow弹出的位置
	pw.setAsDropDown(View view);

>

AlertDialog的构造方法全部是Protected的，所以不能直接通过new一个AlertDialog来创建出一个AlertDialog。

要创建一个AlertDialog，就要用到AlertDialog.Builder中的create()方法。

使用AlertDialog.Builder创建对话框需要了解以下几个方法：

>

	setTitle ：为对话框设置标题
	setIcon ：为对话框设置图标
	setMessage：为对话框设置内容
	setView ： 给对话框设置自定义样式
	setItems ：设置对话框要显示的一个list，一般用于显示几个命令时
	setMultiChoiceItems ：用来设置对话框显示一系列的复选框
	setNeutralButton ：普通按钮
	setPositiveButton ：给对话框添加"Yes"按钮
	setNegativeButton ：对话框添加"No"按钮
	create ： 创建对话框
	show ：显示对话框

>

	Dialog alertDialog = new AlertDialog.Builder(this). 
	setTitle("对话框的标题"). 
	setMessage("对话框的内容"). 
	setIcon(R.drawable.ic_launcher). 
	create(); 
	alertDialog.show();

