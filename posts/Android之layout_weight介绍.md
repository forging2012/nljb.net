---
title: Android之layout_weight介绍
date: '2015-06-23'
description:
categories:

tags:android

---

>

### 追加

>

### weightSum

>

	在LinearLayout的XML中，android:weightSum=“5”，表示这个LinearLayout总共平分成5块大小区域；

	然后再LinearLayout里面的控件，使用android:layout_wetght=“1”，这表示它占用整个布局的1/5。

>

	也就是，父容器使用android:weightSum平分块，然后在子容器中使用layout_wetght设置该容器占用的比例

>

	Layout_开头都是交给父容器，没有Layout_开头都是本身的属性

>

---

>

### layout_weight

>

首先看一下Layout_weight属性的作用：它是用来分配剩余空间的一个属性，你可以设置他的权重。

>

转自：http://blog.csdn.net/xiechengfa/article/details/38334327 有修正, 感谢

>

	<?xml version="1.0" encoding="utf-8"?>     
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"     
	    android:orientation="vertical"     
	    android:layout_width="fill_parent"     
	    android:layout_height="fill_parent"     
	    >     
	<EditText     
	    android:layout_width="fill_parent"     
	    android:layout_height="wrap_content"     
	    android:gravity="left"     
	    android:text="one"/>     
	<EditText     
	    android:layout_width="fill_parent"     
	    android:layout_height="wrap_content"     
	    android:gravity="center"     
	    android:layout_weight="1.0"     
	    android:text="two"/>     
	    <EditText     
	    android:layout_width="fill_parent"     
	    android:layout_height="wrap_content"     
	    android:gravity="right"     
	    android:text="three"/>     
	</LinearLayout>   

>

<img src="{{urls.media}}/Android之layout_weight介绍/1.png" alt="" width="500" hight="800" >

>

看上面代码：

	只有Button2使用了Layout_weight属性，并赋值为了1，而Button1和Button3没有设置Layout_weight这个属性，根据API，可知，他们默认是0

>

下面我就来讲，Layout_weight这个属性的真正的意思：

	Android系统先按照你设置的3个Button高度Layout_height值wrap_content,给你分配好他们3个的高度

	然后会把剩下来的屏幕空间全部赋给Button2,因为只有他的权重值是1，这也是为什么Button2占了那么大的一块空间。

	有了以上的理解我们就可以对网上关于Layout_weight这个属性更让人费解的效果有一个清晰的认识了。

>

---

>

	<？xml version="1.0" encoding="UTF-8"？>   
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"   
	    android:layout_width="fill_parent"   
	    android:layout_height="wrap_content"   
	    android:orientation="horizontal" >   
	    <TextView   
		android:background="＃ff0000"   
		android:layout_width="**"   
		android:layout_height="wrap_content"   
		android:text="1"   
		android:textColor="＠android:color/white"   
		android:layout_weight="1"/>   
	    <TextView   
		android:background="＃cccccc"   
		android:layout_width="**"   
		android:layout_height="wrap_content"   
		android:text="2"   
		android:textColor="＠android:color/black"   
		android:layout_weight="2" />   
	     <TextView   
		android:background="＃ddaacc"   
		android:layout_width="**"   
		android:layout_height="wrap_content"   
		android:text="3"   
		android:textColor="＠android:color/black"   
		android:layout_weight="3" />   
	</LinearLayout> 


>

三个文本框的都是 layout_width=“wrap_content ”时，会得到以下效果

>

<img src="{{urls.media}}/Android之layout_weight介绍/2.jpg" alt="" width="400" hight="100" >

>

按照上面的理解，系统先给3个TextView分配他们的宽度值wrap_content（宽度足以包含他们的内容1,2,3即可）

然后会把剩下来的屏幕空间按照1:2:3的比列分配给3个textview，所以就出现了上面的图像。

>

而当layout_width=“fill_parent”时，如果分别给三个TextView设置他们的Layout_weight为1、2、2的话，就会出现下面的效果：

>

<img src="{{urls.media}}/Android之layout_weight介绍/3.jpg" alt="" width="400" hight="100" >

>

你会发现1的权重小，反而分的多了，这是为什么呢？？？

网上很多人说是当layout_width=“fill_parent”时，weighth值越小权重越大，优先级越高，就好像在背口诀
