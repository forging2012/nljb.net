---
title: mymysql与go-mysql-driver性能比较
date: '2015-05-22'
description:
categories:

tags:system

---

>

转自网络的比较文章

>

mymysql和go-mysql-driver是两个现在都很流行的go的mysql驱动

	// 两个mysql驱动的下载地址：
	https://github.com/ziutek/mymysql
	http://code.google.com/p/go-mysql-driver/>

>

---

>

mymysql和go-mysql-driver的测试总结

>

	1 go-mysql-driver是实现了golang标准库database/sql的产物。底层实现比较有保证
	2 go-mysql-driver虽然每个命令的运行时间比mymysql长，但是内存使用少得非常明显，这点两方算打平。
	3 go-mysql-driver实现了database/sql，如果数据库换成其他的话，不需要更改应用逻辑的代码。
	4 go-mysql-driver实现了database/sql，这个接口的设计也是非常好的，基本和php中的pdo一样，上手和学习成本低。

