---
title: Golang-int-转-string
date: '2014-07-04'
description:
categories:

tags:golang

---

go语言中int类型和string类型都是属于基本数据类型

两种类型的转化都非常简单

下面为大家提供两种int类型转化成string类型的方法！

go语言的类型转化都在strconv package里面，详情请参考：

	http://golang.org/pkg/strconv

下面附上转化代码:

	package main  
	  
	import (  
	    "fmt"  
	    "strconv"  
	)  
	  
	var i int = 10  
	  
	func main() {  
	    // 通过Itoa方法转换  
	    str1 := strconv.Itoa(i)  
	  
	    // 通过Sprintf方法转换  
	    str2 := fmt.Sprintf("%d", i)  
	  
	    // 打印str1  
	    fmt.Println(str1)  
	    // 打印str2  
	    fmt.Println(str2)  
	}  

	%d代表Integer

