---
title: Golang-too_many_open_files-解决方法
date: '2014-07-04'
description:
categories:

tags:golang

---

这是系统资源限制，通常单进程不能超过 1024，我使用cgo来设置，代码如下：

	package main
	 
	/*
	#include <stdio.h>
	#include <sys/time.h>
	#include <sys/resource.h>
	 
	int rlimit_init() {
	    printf("setting rlimit\n");
	 
	    struct rlimit limit;
	 
	    if (getrlimit(RLIMIT_NOFILE, &limit) == -1) {
		printf("getrlimit error\n");
		return 1;
	    }
	 
	    limit.rlim_cur = limit.rlim_max = 50000;
	 
	    if (setrlimit(RLIMIT_NOFILE, &limit) == -1) {
		printf("setrlimit error\n");
		return 1;
	    }
	 
	    printf("set limit ok\n");
	    return 0;
	}
	*/
	import "C"
	 
	func main() {
	    C.rlimit_init()
	}

或者使用 syscall 包

	var rlim syscall.Rlimit
	err := syscall.Getrlimit(syscall.RLIMIT_NOFILE, &rlim)
	if err != nil {
	    fmt.Println("get rlimit error: " + err.Error())
	    os.Exit(1)
	}
	rlim.Cur = 50000
	rlim.Max = 50000
	err = syscall.Setrlimit(syscall.RLIMIT_NOFILE, &rlim)
	if err != nil {
	    fmt.Println("set rlimit error: " + err.Error())
	    os.Exit(1)
	}

使用 go build 编译后，需要以 root 权限运行。
