---
title: Android之WebSocket使用方法
date: '2015-03-31'
description:
categories:

tags:android

---

>

### WebSocket

>

WebSocket protocol 是HTML5一种新的协议。

它实现了浏览器与服务器全双工通信(full-duplex)。

>

Android下支持WebSocket的库

	// 试了很多的库，只有一个可以使用
	支持 https://github.com/codebutler/android-websockets
	失败 https://github.com/tavendo/AutobahnAndroid
	失败 https://github.com/koush/AndroidAsync
	失败 https://github.com/anismiles/websocket-android-phonegap
	失败 https://github.com/TooTallNate/Java-WebSocket
	...
	
>

	// 官方案例
	List<BasicNameValuePair> extraHeaders = Arrays.asList(
	    new BasicNameValuePair("Cookie", "session=abcd");
	);

	WebSocketClient client = new WebSocketClient(URI.create("wss://irccloud.com"), new WebSocketClient.Handler() {
	    @Override
	    public void onConnect() {
		Log.d(TAG, "Connected!");
	    }

	    @Override
	    public void onMessage(String message) {
		Log.d(TAG, String.format("Got string message! %s", message));
	    }

	    @Override
	    public void onMessage(byte[] data) {
		Log.d(TAG, String.format("Got binary message! %s", toHexString(data));
	    }

	    @Override
	    public void onDisconnect(int code, String reason) {
		Log.d(TAG, String.format("Disconnected! Code: %d Reason: %s", code, reason));
	    }

	    @Override
	    public void onError(Exception error) {
		Log.e(TAG, "Error!", error);
	    }
	}, extraHeaders);

	client.connect();

	// Later… 
	client.send("hello!");
	client.send(new byte[] { 0xDE, 0xAD, 0xBE, 0xEF });
	client.disconnect();


