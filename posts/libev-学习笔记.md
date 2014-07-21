---
title: libev-学习笔记
date: '2014-07-21'
description:
categories:

tags:c++

---

针对(ev_io)分析理解

Libev是一个event loop:向libev注册感兴趣的events

比如Socket可读事件,libev会对所注册的事件的源进行管理,并在事件发生时触发相应的程序。

---

	// ev_default_loop,初始化一个不是线程安全的loop
	// ev_loop_new,初始化一个线程安全但不能捕捉信号和子进程的监视
	struct ev_loop *loop = ev_default_loop(0);

>

	// 建立一个(ev_io)结构
	struct ev_io ev;

>

	// 其中(libev)定义了如下类型的:
	ev_io
	ev_timer
	ev_periodic
	ev_signal
	ev_child
	ev_stat
	ev_idle
	ev_prepare and ev_check
	ev_embed
	ev_fork
	ev_cleanup
	ev_async

---

	// 初始化 
	ev_io_init(ev, cb, fd, events);

>

	// 其中,(ev)为(ev_io)结构
	
>

	// 当标准输入上有可读取的数据时，将调用下面这个回调函数
	// 其中,(cb)为回调:
	void cb(struct ev_loop *loop, struct ev_io *watcher, int revents);

>

	// 其中,(fd)为文件描述符(STDIN_FILENO)

>

	// 其中,(events)为(EV_READ)-(EV_IO),也可以为以下类型:

	/* eventmask, revents, events... */
	enum {
	  EV_UNDEF    = (int)0xFFFFFFFF, /* guaranteed to be invalid */
	  EV_NONE     =            0x00, /* no events */
	  EV_READ     =            0x01, /* ev_io detected read will not block */
	  EV_WRITE    =            0x02, /* ev_io detected write will not block */
	  EV__IOFDSET =            0x80, /* internal use only */
	  EV_IO       =         EV_READ, /* alias for type-detection */
	  EV_TIMER    =      0x00000100, /* timer timed out */
	#if EV_COMPAT3
	  EV_TIMEOUT  =        EV_TIMER, /* pre 4.0 API compatibility */
	#endif
	  EV_PERIODIC =      0x00000200, /* periodic timer timed out */
	  EV_SIGNAL   =      0x00000400, /* signal was received */
	  EV_CHILD    =      0x00000800, /* child/pid had status change */
	  EV_STAT     =      0x00001000, /* stat data changed */
	  EV_IDLE     =      0x00002000, /* event loop is idling */
	  EV_PREPARE  =      0x00004000, /* event loop about to poll */
	  EV_CHECK    =      0x00008000, /* event loop finished poll */
	  EV_EMBED    =      0x00010000, /* embedded event loop needs sweep */
	  EV_FORK     =      0x00020000, /* event loop resumed in child */
	  EV_CLEANUP  =      0x00040000, /* event loop resumed in child */
	  EV_ASYNC    =      0x00080000, /* async intra-loop signal */
	  EV_CUSTOM   =      0x01000000, /* for use by user code */
	  EV_ERROR    = (int)0x80000000  /* sent when an error occurs */
	};

---

	// 执行
	ev_io_start(loop, ev);

---

	// 守护
	while (1)
	{
	    // 开始事件循环
	    ev_loop(loop, 0);
	}

---

	// 而且还可以在回调函数内增加事件
	void cb(struct ev_loop *loop, struct ev_io *watcher, int revents)
	{

	    // ------------------------ //
	    // (ev_io)结构创建说明

	    // 一,全局使用一个(ev_io)
	    struct ev_io ev;

	    // 二,对每次子事件分配不同的(ev_io)
	    ev_io *ev = new(ev_io);

	    // ------------------------ //

	    // 错误处理
	    if(EV_ERROR & revents)
		return;

	    ev_io_init(ev, cb, fd, events);
	    ev_io_start(loop, ev);

	}

---

	// 针对Libev实现Socket事件
	#include <iostream>
	#include <ev.h>
	#include <netinet/in.h>
	#include <stdio.h>
	#include <string.h>
	#include <unistd.h>

	using namespace std;

	#define PORT 9999
	#define BUFFER_SIZE 1024

	// accept,事件的回调
	void accept_cb(struct ev_loop *loop, struct ev_io *watcher, int revents);

	// read,事件的回调
	void read_cb(struct ev_loop *loop, struct ev_io *watcher, int revents);

	// 上帝在此
	int main()
	{

	    /* ev_default_loop,初始化一个不是线程安全的loop
	       ev_loop_new,初始化一个线程安全但不能捕捉信号和子进程的监视 */
	    struct ev_loop *loop = ev_default_loop(0);

	    // STDIN_FILENO
	    int sd;

	    // IO,事件
	    struct ev_io socket_accept;

	    // ----------------------------------------------- //

	    // 地址结构,socket
	    struct sockaddr_in addr;

	    // 建立,socekt
	    if( (sd = socket(PF_INET, SOCK_STREAM, 0)) < 0 )
	    {
		printf("socket error");
		return -1;
	    }

	    // ----------------------------------------------- //

	    // 置零
	    bzero(&addr, sizeof(addr));

	    // 地址族
	    addr.sin_family = AF_INET;

	    /* 必须要采用网络数据格式
	       普通数字可以用htons()函数转换成网络数据格式的数字 */
	    addr.sin_port = htons(PORT);

	    /* IP,地址结构
	       INADDR_ANY,就是指定地址为0.0.0.0的地址 */
	    addr.sin_addr.s_addr = INADDR_ANY;

	    // ----------------------------------------------- //

	    // 绑定
	    if (bind(sd, (struct sockaddr*) &addr, sizeof(addr)) != 0)
	    {
		printf("bind error");
		return -1;
	    }

	    // 建立监听,及设置请求连接队列的最大长度,用于限制排队请求的个数
	    if (listen(sd, 5) < 0)
	    {
		printf("listen error");
		return -1;
	    }

	    // ----------------------------------------------- /

	    // 开始监听了io事件
	    ev_io_init(&socket_accept, accept_cb, sd, EV_IO);
	    ev_io_start(loop, &socket_accept);

	    // 循环
	    while (1)
	    {
		// 开始事件循环
		ev_loop(loop, 0);
	    }

	    // 返回
	    return 0;

	}

	// accept 事件的回调
	void accept_cb(struct ev_loop *loop, struct ev_io *watcher, int revents)
	{

	    // 地址结构,socket
	    struct sockaddr_in client_addr;

	    // 地址结构长度,socket
	    socklen_t client_len = sizeof(client_addr);

	    // STDIN_FILENO
	    int client_sd;

	    // 分派客户端的(ev_io)结构
	    ev_io *w_client = new(ev_io);

	    // libev,错误处理
	    if(EV_ERROR & revents)
	    {
		printf("error event in accept");
		return;
	    }

	    // accept,普通写法
	    client_sd = accept(watcher->fd, (struct sockaddr *)&client_addr, &client_len);
	    if (client_sd < 0)
	    {
		printf("accept error");
		return;
	    }

	    printf("someone connected.\n");

	    // 开始监听读事件了,有客户端信息就会被监听到
	    ev_io_init(w_client, read_cb, client_sd, EV_IO);
	    ev_io_start(loop, w_client);


	}

	// read 事件的回调
	void read_cb(struct ev_loop *loop, struct ev_io *watcher, int revents){

	    // 缓冲区
	    char buffer[BUFFER_SIZE];

	    // 读取到的字节数
	    ssize_t read;

	    // libev,错误处理
	    if(EV_ERROR & revents)
	    {
		printf("error event in read");
		return;
	    }

	    // recv普通socket写法
	    read = recv(watcher->fd, buffer, BUFFER_SIZE, 0);

	    if(read < 0)
	    {
		printf("read error");
		return;
	    }


	    // 断开链接的处理,停掉evnet就可以,同时记得释放客户端的结构体!
	    if(read == 0)
	    {
		printf("someone disconnected.\n");
		// 断开连接
		close(watcher->fd);
		// 每一次事件都必须用对应的停止函数，手动停止
		ev_io_stop(loop, watcher);
		// 释放内存
		delete(watcher);
		// 跳出
		return;
	    }
	    else
	    {
		printf("get the message:%s\n",buffer);
	    }

	    // 原信息返回,也可以自己写信息,都一样.
	    send(watcher->fd, buffer, read, 0);

	    // 最后记得置零
	    bzero(buffer, read);

	}
