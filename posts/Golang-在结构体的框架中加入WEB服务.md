---
title: Golang-在结构体的框架中加入WEB服务
date: '2014-08-08'
description:
categories:

tags:golang

---

	// 当你创建了一个结构体,并拥有结构体方法,例如
	type Server struct {
		addr string
	}

	func (this *Server) system() {
	}

---

	// 当你想方便的在WEB中使用全局信息,例如(addr)时
	// 在结构中加入WEB服务是最好的办法
	// 对于属于你的结构体创建WEB只有一个要求
	// 那就是指定给http.ListenAndServe的结构里面有ServeHTTP方法

	func (this *Server) Web(ch chan int) {
		// 循环保护
		for {
			// 开始监听Web请求
			log.Printf("web api listen [%s]", this.addr)
			// 建立监听
			if e := http.ListenAndServe(":"+this.addr, this); e != nil {
				// 输出错误
				log.Printf("web api exception [%s]", e.Error())
				// 休息一下,别太频繁
				time.Sleep(1 * time.Second)
			}
		}

	}

	// Web API 的主接口方法
	func (this *Server) ServeHTTP(w http.ResponseWriter, r *http.Request) {
		// 设置路由
		switch r.URL.Path {
		// 路由接口
		case "/api/snapshot":
			ma.WebSnapshot(w, r)
		default:
			http.NotFound(w, r)
		}
	}

	// 可以在 Server 里面为所欲为的增加方法了
	// 截图
	func (this *Server) WebSnapshot(w http.ResponseWriter, r *http.Request) {
		// 发现请求
		log.Printf("web snapshot find request from [%s]", r.RemoteAddr)
		// 返回结构
		resp := new(WebResp)
		// 初始化参数
		resp.OK = true
		// 执行
		data, err := this.system()
		// 判断是否执行异常
		if err != nil {
			// 返回状态
			resp.OK = false
			resp.Data = err.Error()
			ResponseWriter(w, resp)
			return
		}
		// 状态
		resp.Data = data
		// 返回
		ResponseWriter(w, resp)
	}
		

---

	// 你可能没有发现上面的优点,那看看普通的HTTP如何创建吧

	package main
	 
	import (
	    "net/http"
	)
	 
	func main() {
	    http.HandleFunc("/admin/", adminHandler)
	    http.HandleFunc("/login/",loginHandler)
	    http.HandleFunc("/ajax/",ajaxHandler)
	    http.HandleFunc("/",NotFoundHandler)
	    http.ListenAndServe(":8888", nil)	
	}
