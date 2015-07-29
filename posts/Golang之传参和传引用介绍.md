---
title: Golang之传参和传引用介绍
date: '2015-07-29'
description:
categories:

tags:golang

---

>

### 对Golang传参和传引用的一些试验

>

### 传参之MAP

>

	m := make(map[string]string)
	fmt.Println("src", m)
	change(m)
	fmt.Println("->", m)
	
	// 改变MAP
	func change(m map[string]string) {
		m["nljb"] = "www.nljb.net"
	}

	// 输出
	src map[]
	->  map[nljb:www.nljb.net]

	// 疑点
	Go在传递MAP的时候通过指针或引用方式
	其数据在传递到函数内修改，原数据也随之改变

>

	// 继续证实
	m := make(map[string]string)
	m["a"] = "b"
	fmt.Println(m)
	change(m)
	fmt.Println(m)

	// 改变
	func change(m map[string]string) {
		// 情况一
		// m = nil	
		// 情况二
		m["x"] = "n"
	}

	// 输出
	情况一：
		map[a:b]
		map[a:b]
	情况二：
		map[a:b]
		map[a:b x:n]

	// 理解
	MAP也是按值传递的，因为将传递过来的M赋值空不会对原数据有任何影响
	修改传递的数据，元数据也随之改变使用M里面存储的是指向目标数据的指针

	// 不解	
	在MAP的时候并没有告诉TA初始化值的数量，所以应该没有默认初始化值
	但是为什么在传递值中增加KEY的时候，原数据也跟着改变 ...

>

---

>

### 传参之Slice

>

	s := make([]string, 1)
	fmt.Println("src", s)
	change(s)
	fmt.Println("->", s)

	// 改变Slice
	func change(s []string) {
		s[0] = "www.nljb.net"
	}

	// 疑点
    Go在传递Slice的时候通过指针或引用方式
    其数据在传递到函数内修改，原数据也随之改变

>

	// 继续证实
	a := []int{1, 2, 3, 0}
	fmt.Println(a)
	change(a)
	fmt.Println(a)

	// 改变
	func change(data []int) {
		// 情况一
		data = nil
		// 情况二
		// data[0] = 5
	}

	// 输出
	情况一：
		[1 2 3 0]
		[1 2 3 0]
	情况二：
		[1 2 3 0]
		[5 2 3 0]

	// 解说
	在将Slice修改为空的时候原数据并没有受到影响
	但是在修改里面的数据的时候，原数据却改变了

	// 理解
	Slice是按值传递的
	Slice中的Values是指针,可以修改指向内容的值
	Slice只能修改Values指向的数据, 其他都不能修改

	// 空想
	有时候会想到，那传递的是值，为什么我添加数据的时候
	原数据也跟着改变了，其实make的时候已经初始化了所有的值 ...

>
