---
title: This Handler class should be static or leaks might occur
date: '2015-11-24'
description:
categories:

tags:android

---

>

### This Handler class should be static or leaks might occur

>

关于Android在GC(垃圾回收)时回收Handler会造成空指针异常

>

Android推荐将Handler变为Static(静态)但这样一来其内部调用的都需要为静态

>

	Handler 类应该应该为static类型，否则有可能造成泄露。
	在程序消息队列中排队的消息保持了对目标Handler类的应用。
	如果Handler是个内部类，那 么它也会保持它所在的外部类的引用。
	为了避免泄露这个外部类，应该将Handler声明为static嵌套类，并且使用对外部类的弱应用。

>

---

>

### 解决方法

>

	static class MyHandler extends android.os.Handler {

		private MainActivity mMainActivity;

		DynamicHandler(MainActivity mainActivity) {
			mMainActivity = mainActivity;
		}

		@Override
		public void handleMessage(Message msg) {
			super.handleMessage(msg);
			// 这里使用 mMainActivity 调用外部方法 ...
		}

	}

	private MyHandler mMyHandler = new MyHandler(this);

>

---

>

参考 http://www.cnblogs.com/jevan/p/3168828.html

>

