---
title: sync.Pool
date: '2014-09-30'
description:
categories:

tags:go

---

Go 1.3 的sync包中加入一个新特性：Pool。

官方文档可以看这里http://golang.org/pkg/sync/#Pool

这个类设计的目的是用来保存和复用临时对象，以减少内存分配，降低CG压力。

	type Pool  
	    func (p *Pool) Get() interface{}  
	    func (p *Pool) Put(x interface{})  
	    New func() interface{}  

Get返回Pool中的任意一个对象。

如果Pool为空，则调用New返回一个新创建的对象。

如果没有设置New，则返回nil。

---

还有一个重要的特性是，放进Pool中的对象，会在说不准什么时候被回收掉。

所以如果事先Put进去100个对象，下次Get的时候发现Pool是空也是有可能的。

不过这个特性的一个好处就在于不用担心Pool会一直增长，因为Go已经帮你在Pool中做了回收机制。

这个清理过程是在每次垃圾回收之前做的。垃圾回收是固定两分钟触发一次。

而且每次清理会将Pool中的所有对象都清理掉！

---

	package main

	import(
	    "sync"
	    "log"
	)

	func main(){

	    // 建立对象
	    var pipe = &sync.Pool{New:func()interface{}{return "Hello,BeiJing"}}

	    // 准备放入的字符串
	    val := "Hello,World!"

	    // 放入
	    pipe.Put(val)

	    // 取出
	    log.Println(pipe.Get())

	    // 再取就没有了,会自动调用NEW
	    log.Println(pipe.Get())

	}

	// 输出
	2014/09/30 15:43:30 Hello,World!
	2014/09/30 15:43:30 Hello,BeiJing


