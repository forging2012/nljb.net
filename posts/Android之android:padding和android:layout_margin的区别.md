---
title: Android之android:padding和android:layout_margin的区别
date: '2015-06-23'
description:
categories:

tags:android

---

>

### android:padding和android:layout_margin

>

<img src="{{urls.media}}/Android之android:padding和android:layout_margin的区别/2.jpg" alt="" width="400" hight="300" >

>

### android:layout_margin就是设置view的上下左右边框的额外空间

>

### android:padding是设置内容相对view的边框的距离

>

在LinearLayout、RelativeLayout、TableLayout中，这2个属性都是设置都是有效的

在FrameLayout中，android:layout_margin是无效的，因为FrameLayout里面的元素都是从左上角开始绘制的

在AbsoluteLayout中，没有android:layout_margin属性

>

---

>

<img src="{{urls.media}}/Android之android:padding和android:layout_margin的区别/1.png" alt="" width="300" hight=“500” >

>

### padding是站在父view的角度描述问题 [ ˈpædɪŋ ]

	它规定它里面的内容必须与这个父view边界的距离。

>

### margin则是站在自己的角度描述问题 [ˈmɑ:dʒɪn]

	规定自己和其他（上下左右）的view之间的距离，如果同一级只有一个view，那么它的效果基本上就和padding一样了
