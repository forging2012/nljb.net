---
title: Android之CardView卡片布局使用方法
date: '2015-04-29'
description:
categories:

tags:android

---

>

### CardView

>

CardView继承至FrameLayout类，可以在一个卡片布局中一致性的显示内容，卡片可以包含圆角和阴影。

>

CardView是一个Layout，可以布局其他View。

>

---

>

<img src="{{urls.media}}/Android之CardView卡片布局使用方法/screenshot.jpeg" alt="" width="400" height="600">

>

---

>

	// 支持库
	com.android.support:cardview-v7:22.0.0

>

---

>

	// 需要导入 
	// <android.support.v7.widget.CardView 
	//	xmlns:card_view="http://schemas.android.com/apk/res-auto"

	CardView常用属性：

	// 阴影的大小
	card_view:cardElevation
	// 阴影最大高度
	card_view:cardMaxElevation
	// 卡片的背景色
	card_view:cardBackgroundColor
	// 卡片的圆角大小
	card_view:cardCornerRadius
	// 卡片内容于边距的间隔
	card_view:contentPadding 
		card_view:contentPaddingBottom
		card_view:contentPaddingTop
		card_view:contentPaddingLeft
		card_view:contentPaddingRight
		card_view:contentPaddingStart
		card_view:contentPaddingEnd
	// 设置内边距，V21+的版本和之前的版本仍旧具有一样的计算方式
	card_view:cardUseCompatPadding 
	// 在V20和之前的版本中添加内边距，这个属性为了防止内容和边角的重叠
	card_view:cardPreventConrerOverlap

>

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">
	    <!-- android:background="@drawable/guide_0" -->


	    <android.support.v7.widget.CardView xmlns:card_view="http://schemas.android.com/apk/res-auto"
		android:layout_width="200dp"
		android:layout_height="wrap_content"
		card_view:cardBackgroundColor="#303069"
		card_view:cardCornerRadius="10dp"
		card_view:cardPreventCornerOverlap="true"
		card_view:cardUseCompatPadding="true"
		card_view:contentPadding="10dp"
		android:layout_gravity="right">

		<TextView
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:text="CardView"
		    android:textSize="26sp"
		    android:textColor="#fffffc31" />

	    </android.support.v7.widget.CardView>

	    <android.support.v7.widget.CardView xmlns:card_view="http://schemas.android.com/apk/res-auto"
		android:layout_width="200dp"
		android:layout_height="wrap_content"
		card_view:cardBackgroundColor="#FF0E63FF"
		card_view:cardCornerRadius="10dp"
		card_view:cardPreventCornerOverlap="true"
		card_view:cardUseCompatPadding="true"
		card_view:contentPadding="10dp">

		<TextView
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:text="CardView"
		    android:textSize="26sp"
		    android:textColor="#ff24ff60" />

	    </android.support.v7.widget.CardView>

	    <android.support.v7.widget.CardView xmlns:card_view="http://schemas.android.com/apk/res-auto"
		android:layout_width="200dp"
		android:layout_height="wrap_content"
		card_view:cardBackgroundColor="#FFFF015C"
		card_view:cardCornerRadius="10dp"
		card_view:cardPreventCornerOverlap="true"
		card_view:cardUseCompatPadding="true"
		card_view:contentPadding="10dp"
		android:layout_gravity="right">

		<TextView
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:text="CardView"
		    android:textSize="26sp"
		    android:textColor="#ffffffff" />

	    </android.support.v7.widget.CardView>


	</LinearLayout>

