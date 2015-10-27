---
title: Android之封装volley的ObjectRequest
date: '2015-10-27'
description:
categories:

tags:android

---

>

### 封装volley的ObjectRequest

>

封装volley的两个数据请求类JsonObjectRequest和StringRequest

>

---

	// SuperJsonRequest
	public class SuperJsonRequest {

		// 委托
		private OnJsonRequestDelegate mOnJsonRequestDelegate;

		// 请求标签
		private String mJsonRequestTag;

		// 委托方法
		public interface OnJsonRequestDelegate {

			void onResponse(JSONObject response);

			void onErrorResponse(VolleyError arg0);

		}

		// 设置该委托
		public void setOnJsonRequestDelegate(OnJsonRequestDelegate m) {
			this.mOnJsonRequestDelegate = m;
		}

		// 停止该请求
		public void stopOnJsonRequest() {
			MainApplication.getHttpQueue().cancelAll(this.mJsonRequestTag);
		}

		// 执行该请求
		public void startOnJsonRequest(String url, JSONObject json, String tag) {

			// 默认POST请求，请求地址，请求参数JSON
			JsonObjectRequest jsonObjectRequest = new JsonObjectRequest(Request.Method.POST, url, json,
					new Response.Listener<JSONObject>() {
						public void onResponse(JSONObject response) {
							// 委托
							mOnJsonRequestDelegate.onResponse(response);
						}
					},
					new Response.ErrorListener() {
						public void onErrorResponse(com.android.volley.VolleyError arg0) {
							// 委托
							mOnJsonRequestDelegate.onErrorResponse(arg0);
						}
					}) {

				@Override
				protected Response<JSONObject> parseNetworkResponse(NetworkResponse response) {
					try {
						String jsonString = new String(response.data, "UTF-8");
						return Response.success(new JSONObject(jsonString),
								HttpHeaderParser.parseCacheHeaders(response));
					} catch (UnsupportedEncodingException e) {
						return Response.error(new ParseError(e));
					} catch (JSONException je) {
						return Response.error(new ParseError(je));
					}
				}

				@Override
				public Map<String, String> getHeaders() {
					Map<String, String> headers = new ArrayMap<>();
					headers.put("Accept", "application/json");
					headers.put("Content-Type", "application/json; charset=UTF-8");
					return headers;
				}

			};
			// 全局标签
			this.mJsonRequestTag = tag;
			// 设置标签
			jsonObjectRequest.setTag(tag);
			// 添加请求
			MainApplication.getHttpQueue().add(jsonObjectRequest);
		}

	}

---

	// SuperStringRequest
	public class SuperStringRequest {

		// 委托
		private OnStringRequestDelegate mOnStringRequestDelegate;

		// 请求标签
		private String mStringRequestTag;

		// 委托方法
		public interface OnStringRequestDelegate {

			void onResponse(String arg0);

			void onErrorResponse(VolleyError arg0);

			Map<String, String> getParams();

		}

		// 设置该委托
		public void setOnStringRequestDelegate(OnStringRequestDelegate m) {
			this.mOnStringRequestDelegate = m;
		}
	
		// 停止该请求
		public void stopOnStringRequest() {
			MainApplication.getHttpQueue().cancelAll(this.mStringRequestTag);
		}

		// 开始该请求
		// Request.Method
		public void startOnStringRequest(String url, String tag) {

			// 通过GET请求，请求地址
			StringRequest stringRequest = new StringRequest(Request.Method.GET, url, new Response.Listener<String>() {
				@Override
				public void onResponse(String arg0) {
					mOnStringRequestDelegate.onResponse(arg0);
				}
			}, new Response.ErrorListener() {
				@Override
				public void onErrorResponse(VolleyError arg0) {
					mOnStringRequestDelegate.onErrorResponse(arg0);
				}
			}) {

				@Override
				protected Map<String, String> getParams() throws AuthFailureError {
					// 返回指定的参数MAP
					return mOnStringRequestDelegate.getParams();
				}

				@Override
				protected Response<String> parseNetworkResponse(NetworkResponse response) {
					try {
						String dataString = new String(response.data, "UTF-8");
						return Response.success(dataString, HttpHeaderParser.parseCacheHeaders(response));
					} catch (UnsupportedEncodingException e) {
						return Response.error(new ParseError(e));
					}
				}

			};
			// 全局标签
			this.mStringRequestTag = tag;
			// 设置标签
			stringRequest.setTag(tag);
			// 添加请求
			MainApplication.getHttpQueue().add(stringRequest);
		}

	}

