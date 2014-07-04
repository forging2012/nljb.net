---
title: Golang-strconv
date: '2014-07-04'
description:
categories:

tags:golang

---

	a:=strconv.FormatFloat(10.100,'f',-1,32) 输出:10.1

	a := strconv.FormatFloat(10.101, 'f', -1, 64) 输出:10.101

	a := strconv.FormatFloat(10.010, 'f', -1, 64) 输出：10.01

	a:=strconv.FormatFloat(10.1,'f',2,64) 输出:10.10

	f 参数可以时e,E,g,G
	-1 代表输出的精度小数点后的位数
	如果是<0的值，则返回最少的位数来表示该数，如果是大于0的则返回对应位数的值
	64 为float的类型，go中float分为32和64位，因此就需要传入32或者64

	golang strconv.ParseInt 是将字符串转换为数字的函数,功能灰常之强大,看的我口水直流.

	func ParseInt(s string, base int, bitSize int) (i int64, err error)

	参数1 数字的字符串形式

	参数2 数字字符串的进制 比如二进制 八进制 十进制 十六进制

	参数3 返回结果的bit大小 也就是int8 int16 int32 int64

代码:

	package main
	     
	import (
	    "strconv"
	)

	func main() {

	    i, err := strconv.ParseInt("123", 10, 32)
	    if err != nil {
		panic(err)
	    }
	    println(i)

	}
