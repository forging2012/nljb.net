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

>

---

>

### gravity

>

属性是对该view 内容的限定．比如一个button 上面的text. 你可以设置该text 在view的靠左，靠右等位置．该属性就干了这个．

>

### layout_gravity

>

是用来设置该view相对与起父view 的位置．比如一个button 在linearlayout里，你想把该button放在靠左, 靠右等位置就可以通过该属性设置．

>

这样就解释了，有什么我们弄个最外布局，然后里面包了几个布局，如果要使这几个布局都靠底

就可以在最外布局的属性里设置gravity=”botton” 因为gravity是对里面的内容起作用．

>
