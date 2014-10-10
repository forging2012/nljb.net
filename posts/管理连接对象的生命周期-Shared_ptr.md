---
title: 管理连接对象的生命周期-Shared_ptr
date: '2014-10-10'
description:
categories:

tags:c++

---
 

	// (shared_ptr)的引用计数器决定了连接对象的生命周期

	#include "core/connection.h"  
	#include <vector>  
	  
	using namespace std;  
	  
	class Client: public Connection<Client> {  
	public:  
	Client(io_service& s);  

---

### 销毁连接对象的条件

	那么什么情况下shared_ptr的引用技术会变成0呢？必须满足下列所有条件：

	1. 如果你不再发起任何异步读/写操作

	因为每一次异步读/写操作都会将Client对象自己的this指针包装成shared_ptr
	通过bind交给boost asio框架，此时框架将持有该shared_ptr，直到读/写完成
	回调我们自己的函数后才会将引用计数器减1。

	2. 如果没有任何其他对象或者容器持有这个shared_ptr。

	实际通信程序为了实现服务器事件通知，我会将所有的Client对象的shared_ptr保存在一个容器中，比如map。
	然后定期的检查这些Client有没有在数据库中有事件要发布，如果有，则调用Client的方法发送数据。
	也会定期检查Client对象代表的连接上的心跳消息
	如果心跳超时，则将share_ptr从map中移除掉，并停止任何读/写的异步操作（也就是上上面第一个条件）
	只有在满足了这两个条件的情况下，shared_ptr会自动销毁Client对象，Client对象内部的成员变量socket也会被销毁。

---

### 一般情况下不需要手动关闭socket

	// 所以，我之前用下面的函数关闭socket一般情况下是不需要的
	void CloseSocket() {    
	   try {    
	     socket.shutdown(tcp::socket::shutdown_both);    
	     socket.close();    
	   } catch (std::exception& e) {    
	     BOOSTER_INFO("Connection") << "thread id: " << this_thread::get_id() << e.what() << endl;    
	   }    
	 }    

---

### 多线程环境下关闭连接导致crash

	如果要使用CloseSocket，多线程环境下必须小心。
	比如我碰到过这样的情况：一个线程检查到了超时，然后调用Client::CloseSocket方法
	同时另一个线程正在该Client上发起了异步的读/写操作，结果进程崩溃了。boost asio不允许这样做。

---

### 异步读/写阻塞导致连接对象无法销毁

	如果就坚持不用CloseSocket行么，我碰到另一种情况，async_write/async_read会阻塞
	结果shared_ptr的引用计数不会为0,所以Client对象无法被销毁。
	那么怎么安全的调用CloseSocket的呢？用strand，下面是代码：

	void Client::ToClose() {  
	  strand_.post(bind(&Client::DoClose, shared_from_this()));  
	}  
	  
	void Client::DoClose() {  
	  CloseSocket();  
	}
  
	其他线程就只需要调用ToClose函数即可。
	由于strand_.post的桥梁作用，CloseSocket会在io_service运行的线程池中被保护。
	就不会出现和async_read/async_write同时被执行的情况。
	在实际编程中我们可能由于很多原因要关闭连接，比如前面说的心跳超时，也有收到了不正确的数据
	或者在asio传递出来的网络错误，又或者是收到了关闭进程的信号。
	无论何种原因，都可以采用上面的方式进行关闭连接。所以原则只有一个，记住shared_ptr的生命周期即可。

---

### 优雅的关闭进程

	如何在这种情况下优雅的退出进程是个问题，否则退出时程序会crash，留下core文件。

	这种情况是：

	1. 线程池中运行这io_service对象，进行异步的读/写操作
	2. 一个线程定期检查数据库中的事件消息，并发送给所有远程终端，同时检查每个连接的心跳超时时间。
	3. 一个全局的map对象保存了所有Client的shared_ptr。


