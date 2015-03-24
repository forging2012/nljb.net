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

>

	// Handler 类的主要作用有两个：
	一，在新启动的线程中发送消息
	二，在主线程中获取，处理消息
	// 可以在 THread 或 AsyncTask 中发送消息

>

---

	// 创建一个 Handler
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

	// 触发 Handler，发送信息
	handler.sendEmptyMessage(1000);

