---
title: Android之网络框架volley和async的使用方法
date: '2015-07-24'
description:
categories:

tags:android

---

>

### volley和async

>

	什么是Voley?汉语译过来就是:齐射;迸发的意思。

	可以看出来,Voley特别适合数据量 不大但是通信频繁的场景。

	Voley是GoogleI/O2013上Google官方发布的一款Android平台上的网络通信库。

	以前的网络请求,要考虑开启线程、内存泄漏、性能等等复杂的问题。

	但是Voley框架已 经帮我们把这些问题处理好了,对外提供了相应的完善的请求API,我们只需要按照要求使 用即可。

>

	Async-http是一款国外的开源框架,作者是loopj。是基于ApacheHtpClient库的。

	可以方便快速高效的进行网络数据请求和发送,文件下载和上传。

>

	也就是，网络小数据请求使用Voley，当下载或上传文件时使用Async-http

>

---

>

### volley

>

	// MyApplication.java
	public class MyApplication extends Application {
		public static RequestQueue queue;

		@Override
		public void onCreate() {
			// TODO Auto-generated method stub
			super.onCreate();
			queue = Volley.newRequestQueue(getApplicationContext());
		}

		public static RequestQueue getHttpQueue() {
			return queue;
		}

	}

>

	/**
	 * 1、Volley的Get和Post请求方式的使用
	 * 
	 * 2、Volley的网络请求队列建立和取消队列请求及Activity周期关联
	 * 
	 * @author Administrator
	 * 
	 */

	public class MainActivity extends Activity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			// volley_Get();
			volley_Post();
		}

		@Override
		protected void onStop() {
			// TODO Auto-generated method stub
			super.onStop();
			MyApplication.getHttpQueue().cancelAll("abcPost");
			MyApplication.getHttpQueue().cancelAll("abcGet");
		}

		private void volley_Post() {
			String url = "http://apis.juhe.cn/mobile/get?";
			StringRequest request = new StringRequest(Method.POST, url,
					new Listener<String>() {

						@Override
						public void onResponse(String arg0) {
							Toast.makeText(MainActivity.this, arg0,
									Toast.LENGTH_LONG).show();
						}
					}, new Response.ErrorListener() {

						@Override
						public void onErrorResponse(VolleyError arg0) {
							Toast.makeText(MainActivity.this, "网络请求失败",
									Toast.LENGTH_LONG).show();
						}
					}) {
				@Override
				protected Map<String, String> getParams() throws AuthFailureError {
					HashMap<String, String> map = new HashMap<String, String>();
					map.put("phone", "13666666666");
					map.put("key", "335adcc4e891ba4e4be6d7534fd54c5d");
					return map;
				}
			};
			request.setTag("abcPost");
			MyApplication.getHttpQueue().add(request);
		}

		private void volley_Get() {
			String url = "http://apis.juhe.cn/mobile/get?phone=13666666666&key=335adcc4e891ba4e4be6d7534fd54c5d";
			StringRequest request = new StringRequest(Method.GET, url,
					new Listener<String>() {

						@Override
						public void onResponse(String arg0) {
							Toast.makeText(MainActivity.this, arg0,
									Toast.LENGTH_LONG).show();
						}
					}, new Response.ErrorListener() {

						@Override
						public void onErrorResponse(VolleyError arg0) {
							Toast.makeText(MainActivity.this, "网络请求失败",
									Toast.LENGTH_LONG).show();
						}
					});
			request.setTag("abcGet");
			MyApplication.getHttpQueue().add(request);
		}
	}

>

---

>

### async

>

	public abstract class NetCallBack extends AsyncHttpResponseHandler {

		@Override
		public void onStart() {
			Log.i("info", "请求开始，弹出进度条框");
			super.onStart();
		}

		@Override
		public void onSuccess(String arg0) {
			Log.i("info", "请求成功，隐藏进度条框：" + arg0);
			onMySuccess(arg0);
			super.onSuccess(arg0);
		}

		@Override
		public void onFailure(Throwable arg0) {
			Log.i("info", "请求失败，隐藏进度条框：" + arg0);
			super.onFailure(arg0);
			onMyFailure(arg0);
		}

		public abstract void onMySuccess(String result);

		public abstract void onMyFailure(Throwable arg0);
	}

>

	public class RequestUtils {
		public static AsyncHttpClient client = new AsyncHttpClient();

		public static void ClientGet(String url, NetCallBack cb) {
			client.get(url, cb);
		}

		public static void ClientPost(String url, RequestParams params,
				NetCallBack cb) {
			client.post(url, params, cb);
		}
	}

>

	/**
	 * 1. Android-async-http的Get和Post请求方式的使用
	 * 
	 * 2.Android-async-http回调逻辑的二次封装
	 * 
	 * @author Administrator
	 * 
	 */
	public class MainActivity extends Activity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			// asynchttpGet();
			asynchttpPost();
		}

		private void asynchttpPost() {
			String url = "http://apis.juhe.cn/mobile/get?";
			RequestParams params = new RequestParams();
			params.put("phone", "13666666666");
			params.put("key", "335adcc4e891ba4e4be6d7534fd54c5d");
			RequestUtils.ClientPost(url, params, new NetCallBack() {

				@Override
				public void onMySuccess(String result) {
					Toast.makeText(MainActivity.this, result, Toast.LENGTH_LONG)
							.show();
				}

				@Override
				public void onMyFailure(Throwable arg0) {
					Toast.makeText(MainActivity.this, "请求失败", Toast.LENGTH_LONG)
							.show();
				}
			});
		}

		private void asynchttpGet() {
			AsyncHttpClient client = new AsyncHttpClient();
			String url = "http://apis.juhe.cn/mobile/get?phone=13666666666&key=335adcc4e891ba4e4be6d7534fd54c5d";
			client.get(url, new AsyncHttpResponseHandler() {
				@Override
				public void onSuccess(String arg0) {
					// TODO Auto-generated method stub
					super.onSuccess(arg0);
					Toast.makeText(MainActivity.this, arg0, Toast.LENGTH_LONG)
							.show();
				}

				@Override
				public void onFailure(Throwable arg0) {
					Toast.makeText(MainActivity.this, "网络请求失败", Toast.LENGTH_LONG)
							.show();
					super.onFailure(arg0);
				}
			});
		}

	}

