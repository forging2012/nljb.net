---
title: 把Go程序变小的办法
date: '2014-07-03'
description:
categories:

tags:golang

---

把Go程序变小的办法是：

	go build -ldflags “-s -w” (go install类似)

	-s去掉符号表（然后panic时候的stack trace就没有任何文件名/行号信息了，这个等价于普通C/C++程序被strip的效果）

	-w去掉DWARF调试信息，得到的程序就不能用gdb调试了。


	比如，server.go是一个简单的http server，用了net/http包。

	$ go build server.go
	$ ls -l server
	-rwxr-xr-x 1 minux staff 4507004 2012-10-25 14:16 server
	$ go build -ldflags “-s -w” server.go 
	$ ls -l server
	-rwxr-xr-x 1 minux staff 2839932 2012-10-25 14:16 server


	-s和-w也可以分开使用，一般来说如果不打算用gdb调试，-w基本没啥损失。

	-s的损失就有点大了。

