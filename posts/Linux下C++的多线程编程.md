---
title: Linux下C++的多线程编程
date: '2014-09-26'
description:
categories:

tags:c++

---

简单的多线程编程

　　Linux系统下的多线程遵循POSIX线程接口，称为pthread。

    编写Linux下的多线程程序，需要使用头文件pthread.h，连接时需要使用库libpthread.a。

    顺便说一下，Linux下pthread的实现是通过系统调用clone()来实现的。

    clone()是Linux所特有的系统调用，它的使用方式类似fork。

	// Threads.cpp
	#include 
	#include 
	#include 
	using namespace std;

	void *thread(void *ptr)
	{
	    for(int i = 0;i < 3;i++) {
		sleep(1);
		cout << "This is a pthread." << endl;
	    }
	    return 0;
	}

	int main() {
	    pthread_t id;
	    int ret = pthread_create(&id, NULL, thread, NULL);
	    if(ret) {
		cout << "Create pthread error!" << endl;
		return 1;
	    }
	    for(int i = 0;i < 3;i++) {
		cout <<  "This is the main process." << endl;
		sleep(1);
	    }
	    pthread_join(id, NULL);
	    return 0;
	}

	我们编译并运行此程序，可以得到如下结果：
	This is the main process.
	This is a pthread.
	This is the main process.
	This is the main process.
	This is a pthread.
	This is a pthread.

	再次运行，我们可能得到如下结果：
	This is a pthread.
	This is the main process.
	This is a pthread.
	This is the main process.
	This is a pthread.
	This is the main process.

	// 前后两次结果不一样，这是两个线程争夺CPU资源的结果。

---

上面的示例中，我们使用到了两个函数，pthread_create和pthread_join

    并声明了一个pthread_t型的变量，它是一个线程的标识符。

函数pthread_create用来创建一个线程

    当创建线程成功时，函数返回0，若不为0则说明创建线程失败

    常见的错误返回代码为EAGAIN和EINVAL。

    前者表示系统限制创建新的线程，例如线程数目过多了；

    后者表示第二个参数代表的线程属性值非法。

    创建线程成功后，新创建的线程则运行参数三和参数四确定的函数，原来的线程则继续运行下一行代码。

函数pthread_join用来等待一个线程的结束

    一个线程的结束有两种途径，一种是象我们上面的例子一样，函数结束了，调用它的线程也就结束了；

    另一种方式是通过函数pthread_exit来实现。

---

