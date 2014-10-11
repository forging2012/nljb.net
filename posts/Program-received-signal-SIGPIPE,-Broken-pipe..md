---
title: Program received signal SIGPIPE, Broken pipe.
date: '2014-10-11'
description:
categories:

tags:c++

---

今天通过send发送消息并通过返回来判断连接是否异常(发送是否成功)

但是程序却自动退出了

---

### GDB查看发现

	Program received signal SIGPIPE, Broken pipe.
	0x0000003a7fcd55f5 in send () from /lib64/libc.so.6

---

### 网上查了一下资料

>

在linux下写socket的程序的时候

如果尝试send到一个disconnected socket上，就会让底层抛出一个SIGPIPE信号。

这个信号的缺省处理方法是退出进程，大多数时候这都不是我们期望的。

因此我们需要重载这个信号的处理方法。调用以下代码，即可安全的屏蔽SIGPIPE：

	struct sigaction sa;
	sa.sa_handler = SIG_IGN;
	sigaction( SIGPIPE, &sa, 0 );

---

### 解决办法很多，也很简单。

	// client中忽略SIGPIPE信号
	signal(SIGPIPE, SIG_IGN);

	// 阻止SIGPIPE信号
	sigset_t set;
	sigemptyset(&set);
	sigaddset(&set, SIGPIPE);
	sigprocmask(SIG_BLOCK, &set, NULL);

	// 为SIGPIPE添加信号处理函数，处理完程序继续执行
	signal(SIGPIPE, pipesig_handler);

