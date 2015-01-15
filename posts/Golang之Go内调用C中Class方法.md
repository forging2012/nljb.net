---
title: Golang之Go内调用C中Class方法
date: '2015-01-15'
description:
categories:

tags:golang

---

### 介绍

>

之前已经解决了，在Go中调用C函数方法，及在C内调用Go函数方法

>

但是今日在调用QT内方法的时候发现，所有方法都是封装在类对象中的.

>

且有些对象无法传递出来，私有对象，私有函数，只能内部调用.

---

### 比如：想在Go里面调用QT的QMessageBox弹出个提示框怎么办

>

	// C
	// 第一步，全部对象
	Doors * w;

	// C	
	// 这里初始化了全局对象(new)
	extern "C" int start()
	{
	    int argc = 0 ;
	    char *argv[] = {};
	    QApplication a(argc, argv);
	    w = new Doors();
	    w->init();
	    w->show();
	    return a.exec();
	}

	// C
	// 第二步，当然在你的 QWidget 对象中定义一个方法
	void Doors::message(const char * _message)
	{
	    QMessageBox::about(this, tr("提示"), tr(_message));
	}

	// C
	// 第三部，在外面定义一个供Go调用的方法
	extern "C" void message(void * p)
	{
	    w->message((char*)p);
	}

	// Go	
	// 第四步，Go内声明
	/*
		extern void message(void *);
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -ldoors -framework QtGui
	import "C"
	import "unsafe"

	// Go
	// 第五步，当然是调用了
	// 提示，一定要在 start() 之后使用哦，也就是(new)后.	
	C.message(unsafe.Pointer(C.CString("您如输入命令不支持!")))

