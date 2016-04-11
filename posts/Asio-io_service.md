---
title: Asio-io_service
date: '2014-10-24'
description:
categories:

tags:

---

### IO模型

io_service对象是asio框架中的调度器，所有异步io事件都是通过它来分发处理的

	// io对象的构造函数中都需要传入一个io_service对象
	asio::io_service io_service;
	asio::ip::tcp::socket socket(io_service);

---

### 在asio框架中，同步的io主要流程如下：

<img src="{{urls.media}}/Asio-io_service/1.jpg" alt="" width="250" height="400">

	1, 应用程序调用IO对象成员函数执行IO操作
	2, IO对象向io_service 提出请求.
	3, io_service 调用操作系统的功能执行连接操作.
	4, 操作系统向io_service 返回执行结果.
	5, io_service将错误的操作结果翻译为boost::system::error_code类型，再传递给IO对象.
	6, 如果操作失败,IO对象抛出boost::system::system_error类型的异常.

---

### 而异步IO的处理流程则有些不同：

<img src="{{urls.media}}/Asio-io_service/2.jpg" alt="" width="350" height="400">

	1, 应用程序调用IO对象成员函数执行IO操作
	2, IO对象请求io_service的服务
	3, io_service 通知操作系统其需要开始一个异步连接.
	4, 操作系统指示连接操作完成, io_service从队列中获取操作结果
	5, 应用程序必须调用io_service::run()以便于接收结果
	6, 调用io_service::run()后,io_service返回一个操作结果,并将其翻译为error_code,传递到事件回调函数中

