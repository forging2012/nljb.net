---
title: Android之LinearLayout中baselineAligned的使用
date: '2015-10-27'
description:
categories:

tags:android

---

>

### baselineAligned

>

Android线性布局中的属性主要的就是控制浮动方向的orientation

>

其他的就是辅助浮动显示的，其中有一个属性控制基线，也就是baselineAligned

>

	1.首先这个基线主要是对可以显示文字的View，如TextView，Button等控件的

	2.这个baseline指的是这个UI控件的baseline--文字距UI控件顶部的偏移量

	3.LinearLayout控件默认有属性android:baselineAligned为true
		如果LinearLayout的orientation为horizontal的话，其中的文字默认是文字对齐的

>

---

>

<img src="{{urls.media}}/Android之LinearLayout中baselineAligned的使用/1.png" alt="" width="350" height="120" >

>

---

	// 其中的baselineAlignedChildIndex指的是其中的第几个子控件按照baseline对齐的。

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:baselineAlignedChildIndex="3"
		android:orientation="horizontal" >

		<TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:layout_marginRight="3dip"
			android:text="String1" />

		<LinearLayout
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:baselineAlignedChildIndex="1"
			android:orientation="vertical" >

			<ImageView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:src="@android:drawable/arrow_up_float" />

			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginRight="5dip"
				android:text="String2" />

			<ImageView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:src="@android:drawable/arrow_down_float" />
		</LinearLayout>

		<LinearLayout
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:baselineAligned="true"
			android:baselineAlignedChildIndex="2"
			android:orientation="vertical" >

			<ImageView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:src="@android:drawable/arrow_up_float" />

			<ImageView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:src="@android:drawable/arrow_down_float" />

			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:layout_marginRight="5dip"
				android:text="String3" />
		</LinearLayout>

		<TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="String4"
			android:textSize="60sp" />

	</LinearLayout>

---


