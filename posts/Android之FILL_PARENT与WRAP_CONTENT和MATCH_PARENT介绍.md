---
title: Android之FILL_PARENT与WRAP_CONTENT和MATCH_PARENT介绍
date: '2015-03-06'
description:
categories:

tags:android

---

### 三个属性都用来适应视图的水平或垂直大小

>

### 一个以视图的内容或尺寸为基础的布局比精确地指定视图范围更加方便。

>

	// 包裹
	wrap_content 
	// 填满
	fill_parent 
	// 填满
	match_parent 

>

### FILL_PARENT

>

	设置一个构件的布局为fill_parent将强制性地使构件扩展，以填充布局单元内尽可能多的空间。

	这跟Windows控件的dockstyle属性大体一致。

	设置一个顶部布局或控件为fill_parent将强制性让它布满整个屏幕。

>

### WRAP_CONTENT

>

	设置一个视图的尺寸为wrap_content将强制性地使视图扩展以显示全部内容。

	以TextView和ImageView控件为例，设置为wrap_content将完整显示其内部的文本和图像。

	布局元素将根据内容更改大小。

	设置一个视图的尺寸为wrap_content大体等同于设置Windows控件的Autosize属性为True。

>

### MATCH_PARENT

	Android2.2中match_parent和fill_parent是一个意思 .两个参数意思一样，match_parent更贴切

	于是从2.2开始两个词都可以用。那么如果考虑低版本的使用情况你就需要用fill_parent了

