---
title: Android之SwipeRefreshLayout手势刷新
date: '2015-04-07'
description:
categories:

tags:android

---

>

### SwipeRefreshLayout

>

SwipeRefreshLayout组件是由SDK提供，已经被用于一些Android自己的应用程序（比如Gmail）的实现。

>

SwipeRefreshLayout组件只接受一个子组件：即需要刷新的那个组件。

它使用一个侦听机制来通知拥有该组件的监听器有刷新事件发生

换句话说我们的Activity必须实现通知的接口。

该Activity负责处理事件刷新和刷新相应的视图。

一旦监听者接收到该事件，就决定了刷新过程中应处理的地方。

如果要展示一个“刷新动画”，它必须调用setRefrshing（true），否则取消动画就调用setRefreshing（false）。

>

	// ...
	<?xml version="1.0" encoding="utf-8"?>
	<android.support.v4.widget.SwipeRefreshLayout
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    android:id="@+id/proxy">

	    <FrameLayout
		android:id="@+id/preference"
		android:layout_width="match_parent"
		android:layout_height="match_parent"></FrameLayout>

	</android.support.v4.widget.SwipeRefreshLayout>

>

	@Override
	protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
	    final SwipeRefreshLayout swipeView = (SwipeRefreshLayout) findViewById(R.id.swipe);
	    final TextView rndNum = (TextView) findViewById(R.id.rndNum);
	    swipeView.setColorScheme(android.R.color.holo_blue_dark,
				 android.R.color.holo_blue_light, 
				 android.R.color.holo_green_light, 
				 android.R.color.holo_green_light);
	    swipeView.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
		@Override
		public void onRefresh() {
		    swipeView.setRefreshing(true);
		    Log.d("Swipe", "Refreshing Number");
		    ( new Handler()).postDelayed(new Runnable() {
			@Override
			public void run() {
			    swipeView.setRefreshing(false);
			    double f = Math.random();
			    rndNum.setText(String.valueOf(f));
			}
		    }, 3000);
		}
	    });
	}

>

---

>

### 补充

>

解决，下拉刷新时手势与其它下拉控件冲突问题

>

	// XML
	<?xml version="1.0" encoding="utf-8"?>
	<com.example.nljb.surpass.MySwipeRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:id="@+id/fragment_preference"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">

	    <FrameLayout
		android:id="@+id/preference"
		android:layout_width="match_parent"
		android:layout_height="match_parent"></FrameLayout>

	</com.example.nljb.surpass.MySwipeRefreshLayout>

>

	// 控制下拉距离来实现大距离刷新，小距离忽略
	public class MySwipeRefreshLayout extends SwipeRefreshLayout {

	    private float mPrev;

	    public MySwipeRefreshLayout(Context context, AttributeSet attrs) {
		super(context, attrs);
	    }

	    @Override
	    public boolean onInterceptTouchEvent(MotionEvent event) {

		switch (event.getAction()) {
		    case MotionEvent.ACTION_DOWN:
			mPrev = MotionEvent.obtain(event).getY();
			break;

		    case MotionEvent.ACTION_MOVE:
			final float e = event.getY();
			float diff = Math.abs(e - mPrev);
			if (diff < 200) {
			    return false;
			}
		}

		return super.onInterceptTouchEvent(event);
	    }

	}

>

---

>

### 下拉刷新时切换重影问题

>

	// 这个没找到什么好办法
	// 网上说的设置背景之类的都无效
	// 解决办法时，当发现用户离开当前页面
	// 的意图时，停止刷新

	// Refresh
	public void startRefresh() {
		refreshStatus = true;
		layout.setRefreshing(true);
	}

	// Refresh
	public void stopRefresh() {
		layout.setRefreshing(false);
		refreshStatus = false;
	}

	// 比如我这里是，当用户点击侧滑箭头时停止刷新
	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		int id = item.getItemId();
		switch (id) {
		    // 判断是否为主按钮点击
		    case android.R.id.home:
			// 拦截刷新状态
			if (refreshStatus) {
			    // 停止刷新
			    stopRefresh();
			}
		}
		return super.onOptionsItemSelected(item);
	}


