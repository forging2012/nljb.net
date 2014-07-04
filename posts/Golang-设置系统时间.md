---
title: Golang-设置系统时间
date: '2014-07-03'
description:
categories:

tags:golang 

---

	// 接收UDP时间广播，并设置系统时间
	func (sl *Slaver) masTimeSync(ch chan int) {
	    // 开始监听广播时间
	    log.Printf("time sync listen [%s]", sl.Node.Port.PortUdpSlaTimeSync)
	    for {
		(func() {
		    // 监听 mas 发来的同步时间
		    lis, err := socket.NewListen("", sl.Node.Port.PortUdpSlaTimeSync, 3).ListenUDP()
		    // 判断监听是否建立成功
		    if err != nil {
			// 异常抛出
			log.Fatalln(err)
		    }
		    // 保证监听正常关闭
		    defer lis.Close()
		    // 循环接收
		    for {
			// 每个时间戳大小不超过32字节
			data := make([]byte, 32)
			// 读取时间戳
			read, addr, err := lis.ReadFromUDP(data)
			// 检查是否接收错误
			if err != nil {
			    // 错误时从新接收
			    continue
			}
			// 判断是否为注册服务器所发
			if addr != nil && strings.HasPrefix(addr.String(), sl.MasAddr) {
			    // 转换远程时间戳
			    l, _ := strconv.ParseInt(fmt.Sprintf("%s", data[0:read]), 10, 64)
			    //// 转换时间格式
			    //time := syscall.NsecToTimeval(l)
			    //// 设置系统时间 "Linux Private Settimeofday"
			    //if err := syscall.Settimeofday(&time); err != nil {
			    //  // 异常抛出
			    //  log.Fatalln(err)
			    //}
			    // 设置到系统
			    cmd := exec.Command("date", "-s", time.Unix(0, l).Format("01/02/2006 15:04:05.999999999"))
			    // 设置
			    cmd.Run()
			}
		    }
		})()
	    }
	}


