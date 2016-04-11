---
title: Golang之CGO与QT相互传参
date: '2014-12-25'
description:
categories:

tags:golang

---

### 暂时还没考虑到，对指针释放的问题 ...

>

<img src="{{urls.media}}/Golang之CGO与QT相互传参/1.png" alt="" width="600">

>

	#include "cgo.h"
	#include "doors.h"
	#include "ui_doors.h"
	#include <QtGui>

	Doors::Doors(QWidget *parent) :
	    QMainWindow(parent),
	    ui(new Ui::Doors)
	{
	    ui->setupUi(this);
	}

	Doors::~Doors()
	{
	    delete ui;
	}

	// 主要是这里，从 request 传入字符串，并返回字符串
	void Doors::on_pushButton_clicked() {
	    string x = "xxxxxx";
	    void * p = request((void*)x.c_str(), x.length());
	    QMessageBox::question(this, "hello", tr((char*)p), QMessageBox::Yes, QMessageBox::No);
	}

<img src="{{urls.media}}/Golang之CGO与QT相互传参/2.png" alt="" width="600">

>

	// CGO 的灵魂，声明一个函数，等待GO定义
	#include <string>

	using namespace std;

	extern "C" inline void * request(void *, int) { /* CGO */ }


<img src="{{urls.media}}/Golang之CGO与QT相互传参/3.png" alt="" width="600">

>

	// Go 内是定义
	package main

	/*
		extern void init();
		extern int start();
		// 修正，此处 export 下无需声明
		// extern void * request(void *, int);
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -ldoors
	import "C"
	import "unsafe"
	import "fmt"

	func main() {
		C.init()
		C.start()
	}

	//export request
	func request(_content unsafe.Pointer, _size C.int) unsafe.Pointer {
		fmt.Println(string(C.GoBytes(_content, _size)))
		content := unsafe.Pointer(C.CString("你好, Golang"))
		defer func() {
			C.free(content)
		}()
		return unsafe.Pointer(content)

	}

