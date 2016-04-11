---
title: Golang之Go1.5生成C动态库
date: '2016-02-04'
description:
categories:

tags:golang

---

>

### Go1.5 生成 C 动态库

>

	不能将单个的包编译成C库；

	项目必须包含main包和main入口函数；

>

	// lib.go
	package main

	import "C"

	//export Hello
	func Hello() string {
		return "Hello"
	}

	func main() { }

>

	生成动态库

	go build -v -x -buildmode=c-shared -o lib.so

	生成静态库：

	go build -v -x -buildmode=c-archive -o lib.a

>

	生成动态库后，当前目录会出现, lib.h 与 lib.so

>

---

>

### Go1.5 生成 Go 动态库

>

	// 貌似暂时不支持MacOS与Windows系统

>

	package lib

	//export Hello
	func Hello() string {
		return "Hello"
	}

>

	// 构建核心基本库
	go install -v -x -buildmode=shared runtime sync/atomic
	// 构建GO动态库
	go install -v -x -buildmode=shared -linkshared 

>

	// 调用
	package main

	import (
		"fmt"
		"lib"
	)

	func main() {
		fmt.Println(lib.Hello())
	}

>

	// 编译
	go build -v -x -linkshared

>
