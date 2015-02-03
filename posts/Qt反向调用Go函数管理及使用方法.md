---
title: Qt反向调用Go函数管理及使用方法
date: '2015-02-03'
description:
categories:

tags:golang

---

<img src="{{urls.media}}/Qt反向调用Go函数管理及使用方法/1.png" alt="" width="600">

---

### 简单说一下

>

在Go中将Go->C函数通过函数指针(void*)传入Qt中进行使用

---

### Go->C传入Qt

>

	// 如果看过我之前几篇介绍，这里就不用多说了吧
	/*
		extern void cgo_init();
		extern int cgo_start();
		extern void drv_cgo_callback(void*, void*);
		static void callback()
		{   
			char * _cgo_connect = "cgo_connect";
			extern int cgo_connect(void*, int); // Go -> C
			drv_cgo_callback(_cgo_connect, &cgo_connect); // 将 Go -> C 传入 Qt

		}
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -lexamples
	import "C"
	import "unsafe"

	func start() {
		// 需要注意的是先通过函数调用Qt里面的初始化函数初始化类，以便存储Go -> C的函数
		C.cgo_init()
		// 将 Go -> C 传入 Qt
		C.callback()	
		// 运行
		C.cgo_start()
	}

	// Go -> C
	//export cgo_connect 
	func cgo_connect(_content unsafe.Pointer, _size C.int) C.int {
		device := string(C.GoBytes(_content, _size))
		log.Println("Go->", device)
		if err := StaticConn.Connect(device, "1.0.1"); err != nil {
			return C.int(0)
		}
		return C.int(1)
	}

---

### Qt 里面看一下

>

	#ifndef CGO_H
	#define CGO_H

	#include <map>
	#include <string>

	using namespace std;

	// 定义将函数指针转换回来的函数类型
	typedef int (*COMMAND_CGO_CONNECT_FUNCTION)(void*, int);
	typedef int (*COMMAND_CGO_CHECKCONN_FUNCTION)();
	typedef void (*COMMAND_CGO_DISCONN_FUNCTION)();
	typedef void (*COMMAND_CGO_COMMAND_FUNCTION)(void*, int);
	typedef void (*COMMAND_CGO_SHORTCUTS_FUNCTION)(void*, int);
	typedef void * (*COMMAND_CGO_MESSAGE_FUNCTION)();
	typedef int (*COMMAND_CGO_GOLINE_FUNCTION)(void*, int);

	// 定义一个类
	class Cgo
	{
	public:
	    Cgo();
	    // 将函数指针存入本类
	    int setCgo(void*, void*);
	public:
	    // 所有需要用到的Go函数
	    COMMAND_CGO_CONNECT_FUNCTION cgo_connect;
	    COMMAND_CGO_CHECKCONN_FUNCTION cgo_checkconn;
	    COMMAND_CGO_DISCONN_FUNCTION cgo_disconn;
	    COMMAND_CGO_COMMAND_FUNCTION cgo_command;
	    COMMAND_CGO_SHORTCUTS_FUNCTION cgo_shortcuts;
	    COMMAND_CGO_MESSAGE_FUNCTION cgo_message;
	    COMMAND_CGO_GOLINE_FUNCTION cgo_goline;
	};

	#endif // CGO_H

---

	// 这里就不多说了.
	#include "cgo.h"
	#include <QString>

	Cgo::Cgo()
	{
	    cgo_connect = 0;
	    cgo_checkconn = 0;
	    cgo_disconn = 0;
	    cgo_command = 0;
	    cgo_shortcuts = 0;
	    cgo_message = 0;
	    cgo_goline = 0;
	}

	int Cgo::setCgo(void* _a, void* _b)
	{
	    if (QString("cgo_connect").compare((char*)_a) == 0)
		cgo_connect = (COMMAND_CGO_CONNECT_FUNCTION)_b;
	    if (QString("cgo_checkconn").compare((char*)_a) == 0)
		cgo_checkconn = (COMMAND_CGO_CHECKCONN_FUNCTION)_b;
	    if (QString("cgo_disconn").compare((char*)_a) == 0)
		cgo_disconn = (COMMAND_CGO_DISCONN_FUNCTION)_b;
	    if (QString("cgo_command").compare((char*)_a) == 0)
		cgo_command = (COMMAND_CGO_COMMAND_FUNCTION)_b;
	    if (QString("cgo_shortcuts").compare((char*)_a) == 0)
		cgo_shortcuts = (COMMAND_CGO_SHORTCUTS_FUNCTION)_b;
	    if (QString("cgo_message").compare((char*)_a) == 0)
		cgo_message = (COMMAND_CGO_MESSAGE_FUNCTION)_b;
	    if (QString("cgo_goline").compare((char*)_a) == 0)
		cgo_goline = (COMMAND_CGO_GOLINE_FUNCTION)_b;
	    return 1;
	}


---

### 主要说一下，函数传递进来，怎么用

>

	// 所有会用到Go函数的类都需要有(Cgo *cgo 和 setCgo(Cgo*))
	class Examples : public QMainWindow
	{
	    Q_OBJECT

	public:
	    explicit Examples(QWidget *parent = 0);
	    ~Examples();

	public:
	    QTimer* timer;
	    Cgo* cgo; // 这里

	public:

	    void sendDisplay(const char *);

	    int setCgo(Cgo*); // 这里

	.........

	// 另一个，QDialog 里面也是一样
	class Connect : public QDialog
	{
	    Q_OBJECT

	public:
	    explicit Connect(QWidget *parent = 0);
	    ~Connect();

	public:
	    Cgo *cgo;

	public:
	    int setCgo(Cgo*);

	.......

	// 至于怎么设置，不用我说了吧
	int Connect::setCgo(Cgo *_a)
	{
	    cgo = _a;
	}

---

也就是说，新建一个Cgo类，把所有Go传入进来的类型都放到里面去，然后传到QT类里面!!!

---

	void Examples::sendMessage()
	{
	    // 这里用了 Examples 里面的 cgo
	    void * p = cgo->cgo_message();
	    QString command = (char*)p;
	    if (!command.isEmpty())
	    {
		QMessageBox::information(this, tr("提示"), tr(command.toLatin1().data()), QMessageBox::Yes);
	    }
	}

	// 这里为了让 Connect 也能用所以传入 cgo
	void Examples::on_connect_triggered()
	{
	    Connect conn(this);
	    conn.setCgo(cgo);
	    conn.exec();
	}

	.......
