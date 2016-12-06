---
title: Golang新特性之动态插件包-plugin
date: '2016-12-06'
description:
categories:

tags:golang

---

>

#### Golang新特性之动态插件包 plugin

>

***新增加的标准库 plugin 提供了初步的插件支持，它允许程序可以在运行的时候动态的加载插件***

>

	// 官方介绍
	https://beta.golang.org/pkg/plugin/

>

	// 将以下代码生成插件库 ...
	go build -buildmode=plugin
	
>

	package main

	// // No C code needed.
	import "C"

	import "fmt"

	var V int

	func F() { fmt.Printf("Hello, number %d\n", V) }
	
>

	// 通过插件包使用插件库 ...

	p, err := plugin.Open("plugin_name.so")
	if err != nil {
		panic(err)
	}
	v, err := p.Lookup("V")
	if err != nil {
		panic(err)
	}
	f, err := p.Lookup("F")
	if err != nil {
		panic(err)
	}
	*v.(*int) = 7
	f.(func())() // prints "Hello, number 7"

>

---

>

***以上只是官方例, 由于1.8版本还处于测试阶段, 还不稳定, 待正式版发布后再进行详细测试***

>
