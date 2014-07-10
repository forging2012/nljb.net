---
title: Shell-删除文件中某一行方法
date: '2014-07-10'
description:
categories:

tags:system

---

如果有一个abc.txt文件，内容是:

	aaa
	bbb
	ccc
	ddd
	eee
	fff

如果要删除aaa，那么脚本可以这样写：

	sed -i '/aaa/d' abc.txt

如果删除的是一个变量的值，假如变量是var，应该写成：

	sed -i '/'"$var"'/d' abc.txt

