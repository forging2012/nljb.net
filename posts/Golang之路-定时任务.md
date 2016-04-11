---
title: Golang之路-定时任务
date: '2014-07-03'
description:
categories:

tags:golang

---

	timer := time.NewTicker(2 * time.Second)
	for {
		select {
		case <-timer.C:
	 
		    go func() {
			 log.Println(time.Now())
		    }()
		}
	 }

