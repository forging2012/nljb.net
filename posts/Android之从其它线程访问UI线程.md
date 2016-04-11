---
title: Android之从其它线程访问UI线程
date: '2015-07-28'
description:
categories:

tags:android

---

>

### 从其它线程访问UI线程

>

	// 解决这个问题有几种方法
	1, Activity.runOnUiThread (Runnable)
	2, View.post (Runable)
	3, View.postDelayed (Runable,long)

>

	// UI线程操作注意事项
	1, 不要阻塞UI线程
	2, 不要在UI线程之外访问Android的UI线程

>

---

>

### View.post 

>

	// 通过View.post方法更新UI线程数据
	public void onClick(View v) {
		new Thread(new Runnable() {
			@Override
			public void run() {
				final Bitmap bitmap = loadImageFromNetwork("http://www.baidu.com/logo.png");
				mImageView.post(new Runnable() {
					@Override
					public void run() {
						mImageView.setImageBitmap(bitmap);
					}
				});
			}
		});
	}

>

	// 注意事项
	随着操作变得越来越复杂，这类代码也会变得很复杂, 很难维护, 为了用工作线程完成复杂的交互处理
	可以考虑在工作线程中使用Handler来处理UI线程分发过来的消息
	当然最好的解决方案也许是, 继承使用异步任务类AsyncTask来处理UI线程分发过来的消息

>

---

>

### 异步任务类

>

异步任务类(AsyncTask)能够适当地，简单地用于UI线程，这个类不需要操作线程(Thread)就可以完成后台任务并将结果返回UI

>

	异步任务的定义：
			一个在后台线程上运行，而其结果实在UI线程上发布的任务，异步任务必须被继承使用
			子类至少覆盖一个方法(doInBackground), 但也常覆盖应外一个方法(onPostExecute)

>

	一个异步任务要用到以下三个泛型类型:
			Params  : 启动任务执行的输入参数
			Progress: 后台任务执行的百分比
			Result  : 后台计算的结果类型
			// Void	: 当一个类型不被使用

>

	// AsyncTask 具体使用方法请看 AsyncTask 篇
	private calss MyTask extends AsyncTask<Void, Void, Void> { ... }

>

	// 异步方式下载任务
	public void onClick(View v) {
		new DownloadImageTask().execute("http://www.baidu.com/image.png");
	}

	// ...
	private class DownloadImageTask extends AsyncTask<String, Void, Bitmap> {
		// ...
		protected Bitmap doInBackground(String... urls) {
			return loadImageFromNetwork(urls[0])
		}
		// ...
		protected void onPostExecute(Bitmap result) {
			mImageView.setImageBitmap(result)
		}
	}
