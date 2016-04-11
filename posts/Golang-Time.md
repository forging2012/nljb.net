---
title: Golang-Time
date: '2014-07-15'
description:
categories:

tags:system

---


Go的time包是标准库中的包之一

不用说，几乎是开发必须用到的包之一。

time包的说明文档在：http://golang.org/pkg/time/

看看godoc文档，最大的数据类型就是Time了，这个Time类型最微小可以表示到nanosecond（微毫秒，十亿份之一秒）

<img src="{{urls.media}}/Golang-Time/1.jpg" alt="" width="600">


Time的比较是使用Before,After和Equal方法。看一眼After：

	func (t Time) After(u Time) bool

很好，返回的是bool类型，是我们所需要的。

Sub方法返回的是两个时间点之间的时间距离，看上图看到它返回的是Duration结构，这个结构的具体类型和操作也在godoc中

Add方法和Sub方法是相反的，获取t0和t1的时间距离d是使用Sub，将t0加d获取t1就是使用Add方法

IsZero方法：Time的zero时间点是January 1, year 1, 00:00:00 UTC，这个函数判断一个时间是否是zero时间点

Local，UTC，Ln是用来显示和计算地区时间的。

下面从几个需求直接看time的使用1 请打出当前时间的时间戳，然后将时间戳格式为年月日时分秒的形式

	package main
	 
	import (
	    "fmt"
	    "time"
	)
	 
	func main() {
	    //时间戳
	    t := time.Now().Unix()
	    fmt.Println(t)
	     
	    //时间戳到具体显示的转化
	    fmt.Println(time.Unix(t, 0).String())
	     
	    //带纳秒的时间戳
	    t = time.Now().UnixNano()
	    fmt.Println(t)
	    fmt.Println("------------------")
	     
	    //基本格式化的时间表示
	    fmt.Println(time.Now().String())
	     
	    fmt.Println(time.Now().Format("2006year 01month 02day"))
	 
	}

显示：

<img src="{{urls.media}}/Golang-Time/2.jpg" alt="" width="600">


特别是Format这个函数，可以好好使用

输出当前星期几？

	package main
	 
	import (
	    "fmt"
	    "time"
	)
	 
	func main() {
	    //时间戳
	    t := time.Now()
	    fmt.Println(t.Weekday().String())
	 
	}

<img src="{{urls.media}}/Golang-Time/3.jpg" alt="" width="600">


文档中对这个Weekday类型就没有说明!!没法，直接看代码可以看到：


<img src="{{urls.media}}/Golang-Time/4.jpg" alt="" width="600">


Weekday有一个String()方法

好了，看到这里外带我们有一个推测：

当一个结构中有定义String()函数的时候，fmt.Println()是会调用String的

例子如下：

	package main
	 
	import (
	    "fmt"
	)
	 
	type MyStruct struct{
	}
	 
	func (d MyStruct)String() string {return "mystruct"}
	 
	func main() {
	    me := new(MyStruct)
	    fmt.Println(me)
	 
	}

<img src="{{urls.media}}/Golang-Time/5.jpg" alt="" width="600">

Go的Time之旅结束！！
