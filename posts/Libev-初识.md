---
title: Libev-初识
date: '2014-07-18'
description:
categories:

tags:c++

---

Libev是一个event loop：事件驱动的库。

>

通过向libev注册感兴趣的events，比如socket可读事件
libev会对所注册的事件的源进行管理并在事件发生时触发相应的回调函数。

>

通过event watcher来注册事件。

>

libev通过分配和注册watcher对不同类型的事件进行监听。

>

不同事件类型的watcher又对应不同的数据类型
watcher的定义模式是struct ev_TYPE或者ev_TYPE，其中TYPE为具体的类型。

>

当前libev定义了如下类型的watcher：

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

>

ev_init对一个watcher的与具体类型无关的部分进行初始化。

ev_io_set对watcher的与io类型相关的部分进行初始化，如果是TYPE类型那么相应的函数就是ev_TYPE_set。

可以采用ev_TYPE_init函数来替代ev_init和ev_TYPE_set。

ev_io_start激活相应的watcher

watcher只有被激活的时候才能接收事件。

ev_io_stop停止已经激活的watcher。

ev_run、ev_break以及ev_loop_default都是event loop控制函数。

event loop定义为struct ev_loop。

有两种类型的event loop，分别是default类型和dynamically created类型，区别是前者支持子进程事件。

ev_default_loop和ev_loop_new函数分别用于创建default类型或者dynamically created类型的event loop。

event_run函数告诉系统应用程序开始对事件进行处理，有事件发生时就调用watcher callbacks。

>

除非调用了ev_break或者不再有active的watcher，否则会一直重复这个过程。

	struct　ev_loop　*ev_default_loop　(unsigned　int　flags)　
	struct　ev_loop　*ev_loop_new　(unsigned　int　flags)　

>

默认初始化一个loop，区别是第一个不是线程安全的，第二个不能捕捉信号和子进程的watcher。

>

参数flags可以为下面几种类型：

	#define EVFLAG_AUTO　　　　0x00000000U /* not quite a mask */ 
	/* flag bits */ 
	#define EVFLAG_NOENV　　　 0x01000000U /* do NOT consult environment */ 
	#define EVFLAG_FORKCHECK　 0x02000000U /* check for a fork in each iteration */ 
	/* method bits to be ored together */ 
	#define EVBACKEND_SELECT　 0x00000001U /* about anywhere */ 
	#define EVBACKEND_POLL　　　 0x00000002U /* !win */ 
	#define EVBACKEND_EPOLL　　  0x00000004U /* linux */ 
	#define EVBACKEND_KQUEUE　   0x00000008U /* bsd */ 
	#define EVBACKEND_DEVPOLL    0x00000010U /* solaris 8 */ /* NYI */ 
	#define EVBACKEND_PORT　　　 0x00000020U /* solaris 10 */

	ev_default_fork　()　
	ev_loop_fork　(loop)
	// 当你在子进程里需要使用libev的函数的之前必须要调用。
	// 区别是第二个函数是当使用ev_loop_new创建的loop时，才用第二个函数,也就是说重用父进程创建的loop。

	ev_loop　(loop,　int　flags)
	// 开始事件循环。

	ev_TYPE_init　(ev_TYPE　*watcher,　callback,　[args])
	// 初始化一个watcher。TYPE也就是libev支持的事件类型，比如io，比如time。

	// 第一个参数为一个watcher,第二个回调函数，第三个句柄，第四个事件类型。包含下面几种：
	#define EV_UNDEF　　　　　　　　　　　 -1 /* guaranteed to be invalid */ 
	#define EV_NONE　　　　　　　　　　 0x00 /* no events */ 
	#define EV_READ　　　　　　　　　　 0x01 /* ev_io detected read will not block */ 
	#define EV_WRITE　　　　　　　　　 0x02 /* ev_io detected write will not block */ 
	#define EV_IOFDSET　　　　　　　 0x80 /* internal use only */ 
	#define EV_TIMEOUT　 	0x00000100 /* timer timed out */ 
	#define EV_PERIODIC 	0x00000200 /* periodic timer timed out */ 
	#define EV_SIGNAL　　 	0x00000400 /* signal was received */ 
	#define EV_CHILD　　　 	0x00000800 /* child/pid had status change */ 
	#define EV_STAT　　　　 0x00001000 /* stat data changed */ 
	#define EV_IDLE　　　　 0x00002000 /* event loop is idling */ 
	#define EV_PREPARE　	0x00004000 /* event loop about to poll */ 
	#define EV_CHECK　　　 	0x00008000 /* event loop finished poll */ 
	#define EV_EMBED　　　 	0x00010000 /* embedded event loop needs sweep */ 
	#define EV_FORK　　　　 0x00020000 /* event loop resumed in child */ 
	#define EV_ASYNC　　　 	0x00040000 /* async intra-loop signal */ 
	#define EV_ERROR　　　 	0x80000000 /* sent when an error occurs */

	ev_TYPE_start (loop *, ev_TYPE *watcher)
	// 启动一个watcher。

---

	#include　<ev.h>　
	#include　<stdio.h>　
	　　
	　　//不同的watcher　
	　　ev_io　stdin_watcher;　
	　　ev_timer　timeout_watcher;　
	　　ev_timer　timeout_watcher_child;　
	　　　
	　　//标准输入的回调函数　
	　　static　void　
	　　stdin_cb　(EV_P_　ev_io　*w,　int　revents)　
	　　{　
	　　　puts　("stdin　ready");　
	　　　ev_io_stop　(EV_A_　w);　
	　　　ev_unloop　(EV_A_　EVUNLOOP_ALL);　
	　　}　
	　　
	　　//父进程的定时器回调函数　
	　　static　void　
	　　timeout_cb　(EV_P_　ev_timer　*w,　int　revents)　
	　　{　
	　　　puts　("timeout");　
	　　　ev_unloop　(EV_A_　EVUNLOOP_ONE);　
	　　}　
	　　//子进程的定时器回调函数　
	　　static　void　
	　　timeout_cb_child　(EV_P_　ev_timer　*w,　int　revents)　
	　　{　
	　　　puts　("child　timeout");　
	　　　ev_unloop　(EV_A_　EVUNLOOP_ONE);　
	　　}　

	int　main　(void)　
	{　
	　　//创建一个backend为select的loop　
	　　struct　ev_loop　*loop　=　ev_loop_new(EVBACKEND_SELECT);　
	　　　
	　　　//初始化并启动父进程的watcher　
	　　ev_timer_init(&timeout_watcher,　timeout_cb,　10,　0.);　
	　　ev_timer_start(loop,　&timeout_watcher);　
	　　switch　(fork())　{　
	　　　　　　　　case　-1:　
	　　　　　　　　　　return　-1;　
	　　　　　　　　case　0:　
	　　　　　　　　　//使用父进程loop。　
	　　　　　　　　　　ev_loop_fork(loop);　
	　　　　　　　　　　//子进程的loop　
	　　　　　　　　　　struct　ev_loop　*loop_child　=　ev_loop_new　(EVBACKEND_SELECT);　
	　　　　　　　　　　ev_io_init　(&stdin_watcher,　stdin_cb,　/*STDIN_FILENO*/　0,　EV_READ);　
	　　　　　　　　　　ev_io_start　(loop,　&stdin_watcher);　
	　　　　　　　　　　ev_timer_init(&timeout_watcher_child,　timeout_cb_child,　5.5,　0.);　
	　　　　　　　　　　ev_timer_start(loop_child,　&timeout_watcher_child);　
	　　　　　　　　　　ev_loop(loop_child,0);　
	　　　}　
	　　　
	　　　//等待事件　
	　　　ev_loop　(loop,　0);　
	　　　return　0;　
	}


