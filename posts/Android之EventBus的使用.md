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
	    public void onEvent(String str) {}

	    // 接收
	    public void onEventMainThread(String str) {}

	    // 接收
	    public void onEventPostThread(String str) {}

	    // 接收
	    public void onEventBackgroundThread(String str) {}

	    // 接收
	    public void onEventAsync(String str) {}

	    @Override
	    protected void onDestroy() {
			super.onDestroy();
			// 取消注册
			EventBus.getDefault().unregister(this);
	    }
	}

>

---

>

### 使用方法

>

	1、onEvent
	2、onEventMainThread
	3、onEventBackgroundThread
	4、onEventAsync

	这四种订阅函数都是使用onEvent开头的，它们的功能稍有不同,在介绍不同之前先介绍两个概念：

	告知观察者事件发生时通过EventBus.post函数实现，这个过程叫做事件的发布
						观察者被告知事件发生叫做事件的接收，是通过下面的订阅函数实现的。

	onEvent:
		如果使用onEvent作为订阅函数，那么该事件在哪个线程发布出来的，onEvent就会在这个线程中运行
		也就是说发布事件和接收事件线程在同一个线程。
		使用这个方法时，在onEvent方法中不能执行耗时操作，如果执行耗时操作容易导致事件分发延迟。

	onEventMainThread:
		如果使用onEventMainThread作为订阅函数，那么不论事件是在哪个线程中发布出来的，onEventMainThread都会在UI线程中执行
		接收事件就会在UI线程中运行，这个在Android中是非常有用的，因为在Android中只能在UI线程中跟新UI
		所以在onEvnetMainThread方法中是不能执行耗时操作的。

	onEventBackground:
		如果使用onEventBackgrond作为订阅函数，那么如果事件是在UI线程中发布出来的
		那么onEventBackground就会在子线程中运行，如果事件本来就是子线程中发布出来的
		那么onEventBackground函数直接在该子线程中执行。

	onEventAsync：
		使用这个函数作为订阅函数，那么无论事件在哪个线程发布，都会创建新的子线程在执行onEventAsync.

>

	// 使用重载机制进行收发不同对象
	public void onEventMainThread(FirstEvent event) {  
		Log.d("harvic", "onEventMainThread收到了消息：" + event.getMsg());  
	}  
	  
	public void onEventMainThread(SecondEvent event) {  
		Log.d("harvic", "onEventMainThread收到了消息：" + event.getMsg());  
	}  
	  
	public void onEvent(ThirdEvent event) {  
		Log.d("harvic", "OnEvent收到了消息：" + event.getMsg());  
	}  


