---
title: IOS之界面布局与AutoLayout技术
date: '2016-03-11'
description:
categories:

tags:ios

---

>

### 界面布局

>

***静态表***

>

*表视图(UITableView)与表视图控制器(UITableViewController)其实是一回事。
表视图控制器是一种只能显示表视图的标准视图控制器，可在表视图占据整个视图时使用这种控制器。
虽然如此，相对于使用标准视图控制器并自行添加表视图，使用表视图控制器除了将自动设置委托和数据源属性外
没有任何其它的优势*

>

*注意：Static Cells模式仅仅适用于表视图控制器(UITableViewController)*

*最基本的设置是Content(内容)属性，它包含两个值：Static Cells和Dynamic Prototypes*

* Static Cells用来显示固定的单元格，内容呈现主要通过Xcode的可视化编程来实现，不需要额外的代码支持。
* Dynamic Prototypes为动态单元格，通过设定一个Cell模板
*  // 然后通过实现datasource接口和delegate接口的一些关键方法，从而动态生成表视图

>

***表单布局***

* 创建 Table View Controller
* 设置 Content 为 Static Cells
* 设置 Style 为 Grouped
* 设置 Table View 的 Sections 数量
* 设置 Sections 的 Rows 和 Header 与 Footer
* ... 设置其它如 Border Style 边框属性等

>

* UITableViewStylePlain 按照普通样式显示
* UITableViewStyleGrouped 按分组样式显示

>

---

>

### 集合视图

>

---

>

### AutoLayout

>

<img src="{{urls.media}}/IOS之界面布局与AutoLayout技术/1.png" alt="" width="230" height="60">

>

* Align：用来设置对齐相关的约束；
* Pin：设置相对大小和位置；
* Resolve Auto Layout Issues：解决 autolayout 问题；
* Resizing Behavior：设置重置大小会如何影响其他对象；

>

***Align（对齐）***

>

<img src="{{urls.media}}/IOS之界面布局与AutoLayout技术/2.png" alt="" width="400" height="320">

>

* Leading Edges：头对齐 
* Trailing Edges：尾对齐 
* Top Edges：顶部对齐 
* Bottom Edges：底部对齐
* Horizontal Centers：水平中心对齐 
* Vertical Centers：垂直中心对齐 
* BaseLines：基准线（默认 View 底部位置）水平对齐，用来对齐有文字的控件，如 UILabel、UIButton 等 

*这些是 SuperView 和 SubView 的对齐，SuperView 是 SubView 的 Container*

* Horizontal Center in Container：View 的水平中心和容器的水平中心的相对距离 
* Vertical Center in Container：View 的垂直中心和容器的垂直中心的相对距离 
* // 在对齐数值的白色输入框内，点击右侧下拉框可以选择 “Use Current Canvas Value”
* // 意思是使用当前 Xib/Storyboard 内的差值

*Update Frames 表示如何更新 frame，有三个选项，默认为 None 不更新*

* None：不更新 frame 
* Items of New Constraints：更新新添加的 frame 
* All Frames in Container：更新容器内所有的 frame 

>

***Pin：设置相对大小和位置***

>

<img src="{{urls.media}}/IOS之界面布局与AutoLayout技术/3.png" alt="" width="300" height="430">

>

* 四个矩形框和四条虚线，矩形框里的数字表示当前的View到最近的View(注意：不是SuperView)边缘的距离
* 在矩形框下面有一行灰色字的可选项“Constrain to margins”
* // 意思是在设置上述约束是相对于 margins(外边距属性) 设置的，而 margin 默认距离是 16
* // 如和上边缘距离 306，加上 16，所以 View 的顶部和它上边最近的 View 的距离是 312。

>

* Width：设置宽度 
* Height：设置高度
* Equal Widths：设置两个同级 View 的宽度关系 
* Equal Heights：设置两个同级 View 的高度关系 
* Aspect Ratio：设置 View 自身宽高比例
* Align：和前面所讲的 Align 一致。
* // 那为什么 Align 还会出现在这边呢？估计和 Pin 有关系，故而也放到这边。 

>

***Resolve Auto Layout Issues：解决 autolayout 问题***

<img src="{{urls.media}}/IOS之界面布局与AutoLayout技术/4.png" alt="" width="400" height="300">

>

* Update Frames：更新 frame  
* Update Constraints：更新约束 
* Add Missing Constraints：添加遗漏的约束 
* Reset to Suggested Constraints：重置约束 
* Clear Constraints：清除约束 

>

***Constraint***

<img src="{{urls.media}}/IOS之界面布局与AutoLayout技术/5.png" alt="" width="300" height="560">

>
