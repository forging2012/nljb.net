---
title: Android之自定义AdapterView的空视图
date: '2015-07-20'
description:
categories:

tags:android

---

>

### 自定义AdapterView的空视图

>

要在AdapterView(ListView, GridView, 诸如此类视图)没有数据时显示自定义的视图

>

// 通过 AdapterView.setEmptyView 方法配置空视图

>

	把要显示的视图跟AdapterView放在同一个布局树中，然后通过函数进行设置。
	AdapterView会根据ListAdapter的isEmpty方法的返回值选择显示自身还是空视图

>

	AdapterView仅仅只是变换两者是否可见的参数，而绝不会在布局树中插入或者删除视图

>

	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState)
		ListView list = (ListView) findViewById(R.id.mylist);
		TextView empty = (TextView) findViewById(R.id.myempty);
		// ... 这里设置空视图
		list.setEmptyView(empty);
		// ...
		// 随后可以继续添加Adapter和数据
	}



