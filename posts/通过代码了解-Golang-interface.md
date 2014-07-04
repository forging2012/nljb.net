---
title: 通过代码了解-Golang-interface
date: '2014-07-04'
description:
categories:

tags:golang

---

	package main

	func main() {
	    var s []interface{} = []string{"left", "right"}
	    for _, x := range s.([]string) {
		println(x)
	    }   
	}

	运行会报错：
	cannot use []string literal (type []string) as type []interface {} in assignment
	invalid type assertion: s.([]string) (non-interface type []interface {} on left)

	package main

	func main() {
	    var s interface{} = []string{"left", "right"}
	    for _, x := range s.([]string) {
		println(x)
	    }
	}
	ok


