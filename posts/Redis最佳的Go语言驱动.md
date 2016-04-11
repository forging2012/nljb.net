---
title: Redis最佳的Go语言驱动
date: '2014-12-09'
description:
categories:

tags:golang

---

在redis的官网，golang驱动有几个，忽然来了兴致，那个才是redis最佳的Go语言驱动？

这些驱动都处于开发的前期，还没有发行正式版，有些已经很久没更新了。

从更新日期来看，Gary Burd的radigo和gosexy的redis最近有更新，而且从他们的README文件来看，他们对redis的支持还不错。

在gosexy的redis源码库中的有个_benchmarks文件，里面就是一些对各个redis的Go驱动的一些简单的性能测试。

简单看了一下，里面的代码就是调用他们各自包中的函数来达到测试的功能。

---

	go get github.com/alphazero/Go-Redis
	go get github.com/simonz05/godis
	go get github.com/garyburd/redigo
	go get github.com/gosexy/redis
	go get cgl.tideland.biz/redis

---

<img src="{{urls.media}}/Redis最佳的Go语言驱动/1.png" alt="" width="600">

---

<img src="{{urls.media}}/Redis最佳的Go语言驱动/2.png" alt="" width="600">

---

测试的数据中，GosexyRedis几乎赢得了所有的测试（除了LRange100输给了GaryburdRedigo），GaryburdRedigo基本上是排老二。

而使用gosexy的数据，除了tcgl，其他4个的数据相差不大

而GaryburdRedigo还是赢得了LRange100测试，说明在数量比较大的list方面，GaryburdRedigo是十分有优势的。

从上面的数据可以知道，set, get, incr,lpush的操作耗时都在40微秒左右，那就是1s里面能够操作25000次左右。
