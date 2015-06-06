---
title: Android之AsyncTask的使用方法
date: '2015-03-17'
description:
categories:

tags:android

---

>

### AsyncTask,是android提供的轻量级的异步类

>

	AsyncTask与Thread的区别是：
		Thread不可以在线程中直接操作UI对象，而需要使用消息机制
		AsyncTask可以在线程中直接操作UI对象，onPostExecute()
		其实AsyncTask是Thread的一个高级封装，拆分了主线程与子线程

>

	可以直接继承AsyncTask,在类中实现异步操作
	并提供接口反馈当前异步执行的程度(可以通过接口实现UI进度更新)
	最后反馈执行的结果给UI主线程.

>

	内部会创建一个进程作用域的线程池来管理要运行的任务
	也就就是说当你调用了 AsyncTask#execute()后，AsyncTask会把任务交给线程池
	由线程池来管理创建Thread和运行Therad。
	对于内部的线程 池不同版本的Android的实现方式是不一样的

>

	// Params 启动任务执行的输入参数，比如HTTP请求的URL。
	// Progress 后台任务执行的百分比。
	// Result 后台执行任务最终返回的结果，比如String。
	new AsyncTask<String, Void, String>() {

	    // 后台执行，比较耗时的操作都可以放在这里。注意这里不能直接操作UI。
	    @Override
	    protected String doInBackground(String... params) {
		try {
		    Thread.sleep(1000);
		} catch (InterruptedException e) {
		    e.printStackTrace();
		}
		return null;
	    }

	    // 这里是最终用户调用Excute时的接口，当任务执行之前开始调用此方法，可以在这里显示进度对话框。
	    @Override
	    protected void onPreExecute() {
		super.onPreExecute();
	    }

	    // 相当于Handler 处理UI的方式，在这里面可以使用在 doInBackground 得到的结果处理操作UI。
	    // 此方法在主线程执行，任务执行的结果作为此方法的参数返回
	    @Override
	    protected void onPostExecute(String s) {
		super.onPostExecute(s);
	    }

	    // 可以使用进度条增加用户体验度。
	    // 此方法在主线程执行，用于显示任务执行的进度。
	    @Override
	    protected void onProgressUpdate(Void... values) {
		super.onProgressUpdate(values);
	    }

	    // 用户调用取消时，要做的操作
	    @Override
	    protected void onCancelled(String s) {
		super.onCancelled(s);
	    }

	    // 用户调用取消时，要做的操作
	    @Override
	    protected void onCancelled() {
		super.onCancelled();
	    }

	}.execute();

	// Thread
	new Thread() {
	    @Override
	    public void run() {
		super.run();
	    }
	}.start();


