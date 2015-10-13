---
title: Android之Handler消息传递机制使用
date: '2015-03-24'
description:
categories:

tags:android

---

>

### Handler 介绍

>

出于性能优化考虑，Android的UI操作并不是线程安全的.

Android定制了一条简单的规则，只允许UI线程修改Activity里的UI组件

Handler存在的意义就是一个消息机制, 可以在一个线程中创建并在另一个线程中触发

>

	// Handler 类的主要作用有两个：
	一，在新启动的线程中发送消息
	二，在主线程中获取，处理消息
	// 可以在 Thread 或 AsyncTask 中发送消息

	// Handler中常用的四个方法：
	sendMessage(Message msg)
	sendEmptyMessage()
	sendMessageDelayed(Message msg, long delayMillis)
	post(Runnable r)
	postDelayed(Runnable r, long delayMillis)
	...

>

	Handler负责发送消息
	Looper负责接收Handler发送的消息，并且把消息回传给Handler自己
	MessageQueue就是一个存储消息的容器

>

---

	// 在UI线程中创建一个android.os.Handler
	private android.os.Handler handler = new android.os.Handler() {
		@Override
		public void handleMessage(Message msg) {
		    switch (msg.what){
			case 1000:
			    showToast("开始");
			    break;
			case 1001:
			    showToast("你好");
			    break;
			case 1002:
			    showToast("再见");
			    break;
		    }
		    super.handleMessage(msg);
		}
	};

	// 通过 Handler 显示 Toast
	public void showToast(String str) {
		Toast.makeText(MainActivity2Activity.this, str, Toast.LENGTH_SHORT).show();
	}

	// 在另外一个线程中出发消息机制
	new Thread() {
		@Override
		public void run() {
			// 触发 Handler，发送信息
			// 这时 Handler, 会通过系统消息机制通知到UI线程中的Handler
			// 这是 Handler, 消息机制会在UI线程中回掉handleMessage方法
			handler.sendEmptyMessage(1000);
		}
	}.start();

---

	// Handler 消息处理
	private Handler handler = new Handler() {
		@Override
		public void handleMessage(Message msg) {
			switch (msg.what) {
				// 消息ID
				case 0x1001:
					// 得到数据
					Object obj = msg.obj;
					break;
			}
			super.handleMessage(msg);
		}
	};

	// 通过Handle发送数据消息
	Message msg = new Message();
	msg.obj = obj; // Object obj;
	msg.what = 0x1001; // int what;
	// 发送消息
	handler.sendMessage(msg);

---

	// HandlerThread
	HandlerThread继承于Thread，所以它本质就是个Thread。
	与普通Thread的差别就在于，主要的作用是建立了一个线程，并且创立了消息队列
	有了自己的looper,可以让我们在自己的线程中分发和处理消息。

	// 创建一个 HandlerThread
	HandlerThread thread = new HandlerThread(“handler thread”)

	// 创建一个 属于 HandlerThread 的 Handler
	Handler handler = new Handler(thread.getLooper()) {
		// 重写, handleMessage
		// 说明, 这个handleMessage是在一个完全独立的线程中运行的
		public void handleMessage(Message msg){
			// …
		}
	}
	// 发送消息
	handler.sendEmptyMessage(1)


