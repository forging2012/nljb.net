---
title: 在Golang中获取系统的磁盘内存占用
date: '2014-07-04'
description:
categories:

tags:golang

---

获取磁盘占用情况(Linux/Mac下有效)

http://wendal.net/2012/1224.html

获取磁盘占用情况(Linux/Mac下有效)

获取内存占用

很明显,Windows下的支持是最弱的, 当然,还能通过调用win32 API的方式获取缺失的信息
Golang的API并非完全跨平台, 正如上述的syscall.Statfs_t结构体,在Windows下是没有的
2013年4月6号更新,windows下获取磁盘空间的方法
通过调用win32 api

	//读取内存状态信息
	func(ev*Ev)GetMemo()(int,int,int){
	    //内存状态文件路径
	    varmeminfostring="/proc/meminfo"
	    //存储各状态信息
	    vartotalint
	    varfreeint
	    varusedint
	    //读取文件
	    evolver.FileRF(meminfo,func(f*os.File){
		//建立文件流
		rd:=bufio.NewReader(f)
		for{
		    //按行读取
		    data,err:=rd.ReadString('\n')
		    iferr==io.EOF{
			break
		    }
		    iferr!=nil{
			break
		    }
		    //判断是否为需要的信息
		    ifstrings.HasPrefix(data,"MemTotal"){
			totalValue:=strings.Split(evolver.Trim(strings.Split(data,":")[1]),"")[0]
			total=evolver.ToInt(totalValue,0)
		    }elseifstrings.HasPrefix(data,"MemFree"){
			freeValue:=strings.Split(evolver.Trim(strings.Split(data,":")[1]),"")[0]
			free=evolver.ToInt(freeValue,0)
		    }
		    //计算以用空间
		    used=total-free
		}
	    })
	    //返回结果
	    returnfree,used,total
	}

