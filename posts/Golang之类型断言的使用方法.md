---
title: Golang之类型断言的使用方法
date: '2015-04-30'
description:
categories:

tags:golang 

---

>

### 类型断言

>

	接口类型的一个极端重要的例子是空接口：

	    interface{},它表示空的方法集合，由于任何值都有零个或者多个方法，所以任何值都可以满足它。

>

	也有人说 Go 的接口是动态类型的，不过这是一种误解。

	    它们是静态类型的：接口类型的变量总是有着相同的静态类型，这个值总是满足空接口，只是存储在接口变量中的值运行时可能被改变。

>

	package main

	import (
		"fmt"
	)

	// 自定义类型
	type Hello struct {
		Print interface{}
	}

	// 类型断言必须真对interface{}类型进行
	// value, ok := Interface.(T)
	func main() {

		// 接口类型
		var Interface interface{}

		// 赋值(自定义类型)
		// Interface = Hello{"World"}

		// 赋值(内置类型)
		// Interface = 2

		// 赋值(内置类型)
		// Interface = "3"

		// 输出
		switch Interface.(type) {
		case Hello:
			fmt.Println(Interface.(Hello).Print.(string))
		case int:
			fmt.Println(Interface.(int))
		case string:
			fmt.Println(Interface.(string))

		}

	}


