---
title: Golang之在Ubuntu系统上安装部署
date: '2017-07-13'
description:
categories:

tags:golang

---

>

### Golang之在Ubuntu系统上安装部署

>

*旧版本的安装*

>

	sudo apt-get install golang-go

	export GOROOT=$HOME/go
	export PATH=$GOROOT/bin:$PATH

>

*从1.4版本以后go需要编译安装, 不想编译又想使用新版本的go怎么办*

>

	// 下载地址：http://www.golangtc.com/download
	go1.8.linux-amd64.tar.gz

	// 把安装包解压到/usr/local目录下
	tar -C /usr/local -xzf go1.8.linux-amd64.tar.gz

	// 注意 GOARCH=amd64 或者 386
	vim ~/.bashrc
	export GOPATH=/opt/go
	export GOROOT=/usr/local/go
	export GOARCH=amd64 
	export GOOS=linux
	export GOBIN=$GOROOT/bin/
	export GOTOOLS=$GOROOT/pkg/tool/
	export PATH=$PATH:$GOBIN:$GOTOOLS

>
