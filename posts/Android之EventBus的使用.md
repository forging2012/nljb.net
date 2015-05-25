---
title: Android之EventBus的使用
date: '2015-05-25'
description:
categories:

tags:android

---

>

### EventBus

>

EventBus是一款针对Android优化的发布/订阅事件总线。

>

主要功能是替代(Intent,Handler,BroadCast)在(Fragment,Activity,Service)线程之间传递消息.

>

优点是开销小，代码更优雅。以及将发送者和接收者解耦。

>

---

>

当在开发一些庞大的的项目时，模块比较多，这个时候为了避免耦合度和保证 APP 的效率

>

会用到 EventBus 等类似的事件总线处理模型。

>

这样可以简化一些数据传输操作，保证APP的简洁，做到高内聚、低耦合。

>

---

>

<img src="{{urls.media}}/Android之EventBus的使用/EventBus-Publish-Subscribe.png" alt="" width="600" hight="200">

>

---

>

### EventBus

>

	源码: https://github.com/greenrobot/EventBus
	Gradle: compile 'de.greenrobot:eventbus:2.4.0'

>

---

>

### 基本用法

>

	// 分订阅，注册，发布，取消注册
	EventBus.getDefault().register(this);
	EventBus.getDefault().register(new MyClass());

	// 三个参数分别是：消息订阅者（接收者），接收方法名，事件类
	EventBus.getDefault().register(this, "setTextA", SetTextAEvent.class);

	// 取消注册
	EventBus.getDefault().unregister(this);
	EventBus.getDefault().unregister(new MyClass());

	// 订阅处理数据
	public void onEventMainThread{}
	public void onEvent(AnyEventType event){}
	onEventPostThread, onEventBackgroundThread, onEventAsync

	// 发布
	EventBus.getDefault().postSticky(new SecondActivityEvent("Message From SecondActivity"));
	EventBus.getDefault().post(new ChangelmgEvent(1));

>

---

>

### Event 

>

	public class Event extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		// 注册
		EventBus.getDefault().register(this);
	    }

	    // 发送
	    private void postData() {
		String str = "Hello World";
		// 发送数据，可以是一个类型或者一个类
		EventBus.getDefault().post(str);
	    }

	    // 接收
	    public void onEvent(String str) {

	    }

	    // 接收
	    public void onEventMainThread(String str) {

	    }

	    // 接收
	    public void onEventPostThread(String str) {

	    }

	    // 接收
	    public void onEventBackgroundThread(String str) {

	    }

	    // 接收
	    public void onEventAsync(String str) {

	    }

	    @Override
	    protected void onDestroy() {
		super.onDestroy();
		// 取消注册
		EventBus.getDefault().unregister(this);
	    }
	}


