---
title: Golang Android
date: '2014-12-29'
description:
categories:

tags:golang

---

声明：本文转自(http://blog.nzlov.com/article/44/golang-android.html) , 感谢作者.

---

随着Golang发布1.4正式版，Android下的开发也可以实现(go/mobile)了

就等1.5版本的ios支持了，可以使用golang跨平台开发游戏了...

---

### 环境准备

---

### Ant

>

下载apache-ant并配置好环境变量。

	$ANT_HOME=antpath //你的ant目录
	$PATH=$ANT_HOME/bin:$PATH

>

### Android

>

下载SDK,android-ndk-r9d(这里之所以不用r10d是因为在测试时ndk源码出现问题，而r9d没有问题)。

>

安装并配置环境变量。

	$ANDROID_HOME=sdkpath //你的sdk目录
	$NDK_ROOT=ndkpath //你的ndk目录
	$PATH=$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools:$NDK_ROOT:$PATH

>

### Go

1.下载Go，或者克隆。

	Git
	$ git clone https://github.com/golang/go.git
	Hg(不推荐使用)
	$ hg clone https://code.google.com/p/go

>

2.配置环境变量

>

	$GOOS=darwin
	$GOARCH=amd64
	$GOROOT=gopath //你的go源码目录
	$GOPATH=goworkpath //你的go工作目录
	$GOBIN=$GOPATH/bin
	$PATH=$GOBIN:$GOROOT/bin:$PATH

>

3.编译 使用console进入go源码目录执行

	$cd $GOROOT/src
	$./all.bash

>

4.测试

	$go version
	go version devel +082a237 Fri Dec 12 04:59:51 2014 +0000 darwin/amd64

---

### 构建Golang Android环境

---

### Android NDK交叉环境构建

>

	$$NDK_ROOT/build/tools/make-standalone-toolchain.sh --platform=android-9 \
	    --install-dir=$NDK_ROOT --system=darwin-x86_64

>

执行结果：

	Auto-config: --toolchain=arm-linux-androideabi-4.6
	Copying prebuilt binaries...
	Copying sysroot headers and libraries...
	Copying libstdc++ headers and libraries...
	Copying files to: /Users/qipeng/program/android/android-ndk-r9d

>

Golang 交叉环境构建

	$cd $GOROOT/src
	$CC_FOR_TARGET=$NDK_ROOT/bin/arm-linux-androideabi-gcc GOOS=android \
	    GOARCH=arm GOARM=7 CGO_ENABLED=1  ./make.bash

>

执行结果：

	Installed Go for android/arm in /Users/qipeng/program/go
	Installed commands in /Users/qipeng/mac/workspace/go/bin

>

测试,使用官方(go/mobile)自带例子测试。

	//由于gfw可能无法下载，可以翻墙也可以从Github上下载并move到golang.org/x/mobile下
	$go get golang.org/x/mobile
	$cd $GOPATH/src/golang.org/x/mobile/example/basic
	$./all.bash //最好连接你的手机并开启usb调试，这样编译完成后会直接在手机上运行。

>

<img src="{{urls.media}}/Golang-Android/1.png" alt="" width="600">




