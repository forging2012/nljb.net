---
title: Golang之CGO与QT绑定编译
date: '2014-12-23'
description:
categories:

tags:golang

---

### 随便写个QT窗口

>

<img src="{{urls.media}}/Golang之CGO与QT绑定编译/1.png" alt="" width="600">

---

### 将代码编译成库文件

>

<img src="{{urls.media}}/Golang之CGO与QT绑定编译/2.png" alt="" width="600">

---

### Go中调用这个函数

>

<img src="{{urls.media}}/Golang之CGO与QT绑定编译/3.png" alt="" width="600">

---

### 完成 

>

<img src="{{urls.media}}/Golang之CGO与QT绑定编译/4.png" alt="" width="600">

---

### 字符串参数传递

>

	#include <QtGui>

	extern "C" void qtDebug(const char *typeName)
	{
	    qDebug() << "Debug:" << typeName;
	}

	extern "C" int start(const char *typeName) {

	    int argc =0;
	    char **argv = 0;
	    QApplication a(argc, argv);
	    QDialog w;
	    QLabel l(&w);
	    l.setText(typeName);
	    w.show();

	    return a.exec();
	}

>

	package main

	/*
		extern int start(const char *typeName);
		extern void qtDebug(const char *typeName);
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -ldemo
	import "C"
	import "unsafe"

	func main() {
		cTypeName := C.CString("abc")
		C.qtDebug(cTypeName)
		C.start(cTypeName)
		C.free(unsafe.Pointer(cTypeName))
	}


---
