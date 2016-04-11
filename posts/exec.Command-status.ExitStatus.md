---
title: exec.Command-status.ExitStatus
date: '2014-07-04'
description:
categories:

tags:golang

---

	// 可用串口
	 
	func (d *Dial) SerialOK(dev []string) string {
	 
	    // 遍历设备
	 
	    for _, v := range dev {
	 
		// 通过调用程序来判断可用串口
	 
		cmd := exec.Command("/danoo/bin/danoo_3g_signal", v)
	 
		// 运行程序
	 
		if err := cmd.Start(); err != nil {
	 
		    continue
	 
		}
	 
		// 线程
	 
		chTime := make(chan int)
	 
		// 飞一会干掉他
	 
		go (func(ch chan int, cmd *exec.Cmd) {
	 
		    // 让程序飞一会
	 
		    time.Sleep(time.Second * 5)
	 
		    // 然后干掉它
		    cmd.Process.Kill()
	 
		    // 返回
	 
		    ch <- 0
	 
		})(chTime, cmd)
	 
		// 等待程序结束
	 
		if err := cmd.Wait(); err != nil {
	 
		    if exiterr, ok := err.(*exec.ExitError); ok {
	 
			if status, ok := exiterr.Sys().(syscall.WaitStatus); ok {
	 
			    if status.ExitStatus() == 99 {
	 
				return v
	 
			    }
	 
			}
	 
		    }
	 
		}
	 
		// 返回锁
	 
		<-chTime
	 
	    }
	 
	    // 返回
	 
	    return ""
	 
	}

