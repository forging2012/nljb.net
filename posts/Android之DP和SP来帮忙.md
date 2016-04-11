---
title: Android之DP和SP来帮忙
date: '2015-05-04'
description:
categories:

tags:android

---

>

### DP和SP

>

为了让程序拥有更好的屏幕适配能力，在指定控件和布局大小的时候

最好使用match_parent和wrap_content尽量避免将控件指定一个固定值

不过在有些时候紧紧使用match_parent和wrap_content确实无法满足需求

这时候必须给控件的宽或高指定一个固定值时，DP和SP应运而生.....

>

---

>

### PX和PT

>

	PX 是像素的意思，既屏幕中可以显示的最小元素单位
	PT 是磅数，1磅等于1/72英寸，一般PT都会作为字体的单位来使用

>

---

>

### 出现的问题

>

	手机上像素各不相同，一个200PX宽的按钮在低分辨率的手机上面可能
	    将近占据满屏，而到了高分辨的手机上可能只占据屏幕的一半

	可以看出，同样的PX和PT在不同分辨率的屏幕上显示的效果是完全不同的
	    这导致这两个单位在手机领域上面很难有所发挥

>

---

>

### 解决方案

>

	谷歌当然也意识到了这个令人头疼的问题，于是Android引入了一套新的单位DP和SP
	
	DP是密度无关像素的意思，也被称作DIP，和PX相比，它在不同密度的屏幕中显示比例保持一致

	SP是可伸缩像素的意思，它采用了和DP同样的设计理念，解决了文字大小适配的问题

>

---

>

### 获取密度

>

	float xdpi = getResources().getDisplayMetrics().xdpi;
	float ydpi = getResources().getDisplayMetrics().ydpi;

