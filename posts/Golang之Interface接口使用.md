---
title: Golang之Interface接口使用
date: '2015-04-17'
description:
categories:

tags:golang

---

>

### Interface

>

	package main

	import (
		"fmt"
	)

	func main() {

		// 创建一个Struct对象并赋值给Interface生成接口对象
		// 只有Struct与Interface存在共性函数才可以生成对象
		// 而该接口对象只保留Struct与Interface的共性函数
		// 多个接口对象来一不同Struct但赋值给同一Interface
		// 它们相互间Interface相同，只是接口相同，实现不同
		var a Interface = new(A)
		var b Interface = new(B)
		Print(a)
		Print(b)

	}

	// Interface
	type Interface interface {
		Close()
	}

	// A
	type A struct {
	}

	// A Function
	func (this *A) Open() {

	}

	// A Function
	func (this *A) Close() {
		fmt.Println("Close is A")
	}

	// B
	type B struct {
	}

	// B Function
	func (this *B) Close() {
		fmt.Println("Close is B")
	}

	// Print
	func Print(i Interface) {
		i.Close()
	}

