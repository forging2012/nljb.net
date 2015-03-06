---
title: Android之Notification通知用法
date: '2015-03-06'
description:
categories:

tags:android

---

当用户有没有接到的电话的时候，Android顶部状态栏里就会出现一个小图标。

提示用户有没有处理的快讯，当拖动状态栏时，可以查看这些快讯。

Android给我们提供了NotificationManager来管理这个状态栏。

可以很轻松的完成。

---

	public void send()
	{
	// .......
	// 消息ID
	final int NOTIFY_ID = 0;
	// 获取系统的 NotificationManager 服务
	NotificationManager notificationManager = (NotificationManager)getSystemService(NOTIFICATION_SERVICE);
	// 创建一个点击消息跳转的 Activity
	Intent intent = new Intent(this, ControlActivity.class);
	// Intent 是及时启动，intent 随所在的activity 消失而消失。 
	// PendingIntent 可以看作是对intent的包装，通常通过getActivity,getBroadcast ,getService来得到pendingintent的实例
	// 当前activity并不能马上启动它所包含的intent,而是在外部执行 pendingintent时，调用intent的
	PendingIntent contentIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);//FLAG_ONE_SHOT
	// 开始创建 Notification
	Notification notification = new Notification.Builder(this)
		// 设置显示在状态栏的通知提示信息
		.setTicker("有新的消息")
		// 设置消息标题
		.setContentTitle("消息标题")
		// 设置消息内容
		.setContentText("消息内容")
		// 通知消息要启动的 Activity
		.setContentIntent(contentIntent)
		// 设置消息图标
		.setSmallIcon(R.mipmap.ic_launcher)
		// 设置该通知自动消失
		.setAutoCancel(true)
		// 设置通知的时间 (消息上面显示的时间)
		.setWhen(System.currentTimeMillis())
		// 使用默认声音，默认LED灯，震动
		// DEFAULT_SOUND 默认声音
		// DEFAULT_VIBRATE 默认震动
		// DEFAULT_LIGHTS 默认闪光灯
		// DEFAULT_ALL 设置使用默认声音，震动，闪光灯
		// 设置声音 .setSound(Uri.pare("file:///sdcard/click.mp3"));
		// 设置震动 .setVibrate(new long[]{0, 50, 100, 150});
		.setDefaults(Notification.DEFAULT_ALL)
		// 为通知设置大图标 .setLargeIcon(...)
		// 生成 Notification
		.getNotification();
	// 发送消息
	notificationManager.notify(NOTIFY_ID, notification);
	// .......
	}

---

而在Android中，如果需要访问硬件设备的话，是需要对其进行授权的

所以需要在清单文件AndroidManifest.xml中增加两个授权，分别授予访问振动器与闪光灯的权限：

	<!-- 闪光灯权限 -->
	<uses-permission android:name="android.permission.FLASHLIGHT"/>
	<!-- 振动器权限 -->
	<uses-permission android:name="android.permission.VIBRATE"/>

