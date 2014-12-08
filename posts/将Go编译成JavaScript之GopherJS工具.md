---
title: 将Go编译成JavaScript之GopherJS工具
date: '2014-12-08'
description:
categories:

tags:golang

---

GopherJS 可以将 Go 代码编译成纯 JavaScript 代码。

其主要目的是为了让你可以使用 Go 来编写前端代码，这些代码可执行在浏览器上运行。

你可以通过这里尝试下 GopherJS： GopherJS Playground.

例如 JavaScript 代码：

	document.write("Hello world!");

用 GopherJS 来写就变成这样：

	js.Global.Get("document").Call("write", "Hello world!")

好像复杂了不少，函数调用这样：

	package main
	 
	import "github.com/gopherjs/gopherjs/js"
	 
	func main() {
	  js.Global.Set("myLibrary", map[string]interface{}{
	    "someFunction": someFunction,
	  })
	}
	 
	func someFunction() {
	  [...]
	}

