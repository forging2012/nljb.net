---
title: Golang之vendor的使用方法
date: '2016-06-07'
description:
categories:

tags:golang

---

>

#### Golang之vendor的使用方法

>

Go第三方包依赖和管理的问题由来已久，民间知名的解决方案就有 godep, gb 等。

这次Go team在推出vendor前已经在Golang-dev group上做了长时间的调研,

最终Russ Cox在 Keith Rarick 的proposal的基础上做了改良,形成了Go 1.5中的vendor

>

---

>

***Go 1.5 中需要***

* export GO15VENDOREXPERIMENT=1 开启
* export GO15VENDOREXPERIMENT=0 关闭

>

	注意：Go 1.6 默认开启

>

***Russ Cox基于前期调研的结果，给出了vendor机制的群众意见基础：***

* 不rewrite gopath
* go tool来解决
* go get兼容
* 可reproduce building process

>

	// 官方例
    d/
        vendor/
              p/
				p.go
        mypkg/
              main.go

    // main.go
    package main
    import "p"
    func main() {
        p.P()
    }

>

    pkg/
    src/
        vendor/
            github.com/system/system.go(package system)
            github.com/system/socket.go(package socket)
        stat.go(import github.com/system/system)
        main.go(import github.com/system/socket)
        ...

>

* 全局共享的import库放到GOPATH路径下
* 项目独享的import放在vendor下
* 需要发送或者打包的项目关联库放到项目的vendor下

>
