---
title: Golang-判断文件是否存在
date: '2014-07-04'
description:
categories:

tags:golang

---

golang判断文件是否存在有点怪异

是判断在操作文件时返回的错误信息来判断的

不能直接根据路径判断,感觉怪异.呵呵

	package main     
	import (
	     "fmt"
	     "os" 
	)
	 
	func main() {

	     f, err := os.Open("dotcoo.com.txt")
	     if err != nil && os.IsNotExist(err) {
		 fmt.Printf("file not exist!\n")
		 return
	     }
	     fmt.Printf("file exist!\n")
	     defer f.Close()

	}

