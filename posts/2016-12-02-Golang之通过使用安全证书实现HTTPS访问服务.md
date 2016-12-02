---
title: Golang之通过使用安全证书实现HTTPS访问服务
date: '2016-12-02'
description:
categories:

tags:golang

---

>

#### 通过使用安全证书实现HTTPS访问服务

>

---

>

**获取证书流程：**

>

* 注册阿里云 https://www.aliyun.com/
* 在（管理控制台 - 安全云顿 - 证书服务）中购买证书
* 注：阿里云提供免费的（免费型DV SSL）证书 ... 
* 在证书补全中添加域名（只支持一个域名），并等待审核
* 审核通过后点击下载证书（下载证书 for Nginx）获取证书

>

---

>

***1. 购买证书***

<img src="{{urls.media}}/2016-12-02-Golang之通过使用安全证书实现HTTPS访问服务/2.png" alt="" width="600">

***2. 补全域名***

<img src="{{urls.media}}/2016-12-02-Golang之通过使用安全证书实现HTTPS访问服务/3.png" alt="" width="600">

***3. 签发完毕***

<img src="{{urls.media}}/2016-12-02-Golang之通过使用安全证书实现HTTPS访问服务/5.png" alt="" width="600">

***4. 下载证书***

<img src="{{urls.media}}/2016-12-02-Golang之通过使用安全证书实现HTTPS访问服务/4.png" alt="" width="600">

>

---

>

**使用证书：**

>

* 解压下载证书获取 $ID.key 和 $ID.pem 证书，将证书保存到自定义的项目目录中 ...

>

    // 配置证书
    package main
    
    import (
    	"io"
    	"log"
    	"net/http"
    )
    
    func helloHandler(w http.ResponseWriter, r *http.Request) {
    	io.WriteString(w, "hello world!")
    }
    
    func main() {
    	http.HandleFunc("/hello", helloHandler)
    	err := http.ListenAndServeTLS(":8080", "213962744560812.pem", "213962744560812.key", nil)
    	if err != nil {
    		log.Fatal("ListenAndServeTLS:", err.Error())
    	}
    }

>

---

>

**测试说明：**

>

***本地测试可以修改hosts文件(127.0.0.1 www.xxx.com)指向证书内的域名 ...***

>

<img src="{{urls.media}}/2016-12-02-Golang之通过使用安全证书实现HTTPS访问服务/1.png" alt="" width="600">

>