---
title: Go之C中声明-Go内定义-C里调用
date: '2014-12-24'
description:
categories:

tags:golang

---

### 说起来容易，做起来难啊，研究了两天了 ...

>

<img src="{{urls.media}}/Go之C内声明-Go内定义-C内调用/1.png" alt="" width="600">

>

	// extern "C" 就不说了
	// inline 多处定义 ... 这样就可以在 Go 中定义了 ...
	extern "C" inline void drv_appmain() { /* CGO */ }

	// 在 start() 函数内, 调用 drv_appmain()
	// 也就是 Go 中的 drv_appmain() 了.
	extern "C" int start() {
	    drv_appmain();
	    int argc = 0;
	    char **argv = 0;
	    QApplication a(argc, argv);
	    QDialog w;
	    QTabWidget t(&w);
	    t.resize(800, 600);
	    QWidget widget_a;
	    QWidget widget_b;
	    widget_a.resize(800,600);
	    widget_b.resize(800,600);
	    t.addTab(&widget_a,  "0");
	    t.addTab(&widget_b,  "1");
	    QMessageBox x;
	    x.show();
	    w.show();
	    return a.exec();
	}

>

<img src="{{urls.media}}/Go之C内声明-Go内定义-C内调用/2.png" alt="" width="600">

>

	// Go中做了什么呢 ...
	// 声明一下，使用的函数 
	// 调用启动函数 start()
	// 这句 //export drv_appmain 是必不可少的 ...
	// 在里面用Go畅所欲言吧...
	// 要注意的是，需要使用 C.int , C.uint ...
	package main

	/*
		extern int start();
		extern void drv_appmain();
	*/
	// #cgo LDFLAGS: -L./ -ldemo
	import "C"
	import "fmt"

	func main() {
		C.start()
	}

	//export drv_appmain
	func drv_appmain() {
		fmt.Println("xxx")
	}



