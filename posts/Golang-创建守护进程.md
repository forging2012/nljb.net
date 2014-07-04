---
title: Golang-创建守护进程
date: '2014-07-04'
description:
categories:

tags:golang

---

	package main

	import (
	    "os"
	    "os/exec"
	    "path/filepath"
	)

	if os.Getppid()!=1{

		//判断当其是否是子进程，当父进程return之后，子进程会被 系统1 号进程接管
		filePath,_:=filepath.Abs(os.Args[0])  //将命令行参数中执行文件路径转换成可用路径
		cmd:=exec.Command(filePath,os.Args[1:]...)//将其他命令传入生成出的进程
		cmd.Stdin=os.Stdin //给新进程设置文件描述符，可以重定向到文件中
		cmd.Stdout=os.Stdout
		cmd.Stderr=os.Stderr
		cmd.Start() //开始执行新进程，不等待新进程退出
		return

	}

这样可以创建守护进程，并且可以接受信号量

另一种方式

	if os.Getppid()!=1{   
		args:=append([]string{filePath},os.Args[1:]...)
		os.StartProcess(filePath,args,&os.ProcAttr{Files:[]*os.File{os.Stdin,os.Stdout,os.Stderr}})
		return
	}

也可以创建守护进程，文档上说startProcess是更底层的接口，cmd是高级的接口

其实还可以更低层

	pid, _, sysErr := syscall.RawSyscall(syscall.SYS_FORK, 0, 0, 0)
		if sysErr != 0 {
		Utils.LogErr(sysErr)
		return
	}

go源码中其实就是这么生成新进程的

直接调用系统函数fork来生成新进程，只是这样不知道为什么总是注册不了信号量处理函数

