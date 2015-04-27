---
title: Android之SwipeLayout实现View侧滑菜单
date: '2015-04-27'
description:
categories:

tags:android

---

>

### AndroidSwipeLayout

>

一个很强大的SwipeLayout和SwipeListView相比它不局限于ListView

>

<img src="{{urls.media}}/Android之SwipeLayout实现View侧滑菜单/swipelayout.gif" alt="" width="300" height="500">

>

官方：https://github.com/daimajia/AndroidSwipeLayout/wiki/usage

>

Jar：https://github.com/daimajia/AndroidSwipeLayout/releases/download/v1.1.8/AndroidSwipeLayout-v1.1.8.jar

>

---

>

Show Mode : 支持 LayDown 和 PullOut

>

Drag edge : 支持 LEFT, RIGHT, TOP, BOTTOM

>

---

>

	// XML
	// 一个SwipeLayou包含两个Layout
	// 其中一个Layout是隐藏的(侧滑才会显示)
	// 另外一个Layout是显示的
	<com.daimajia.swipe.SwipeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="80dp">
	    <!-- Bottom View Start-->	
	     // 隐藏的
	     <LinearLayout
		android:background="#66ddff00"
		android:id="@+id/bottom_wrapper"
		android:layout_width="160dp"
		android:weightSum="1"
		android:layout_height="match_parent">
		<!--What you want to show-->
	    </LinearLayout>
	    <!-- Bottom View End-->

	    <!-- Surface View Start -->
	     // 显示的 
	     <LinearLayout
		android:padding="10dp"
		android:background="#ffffff"
		android:layout_width="match_parent"
		android:layout_height="match_parent">
		<!--What you want to show in SurfaceView-->
	    </LinearLayout>
	    <!-- Surface View End -->
	</com.daimajia.swipe.SwipeLayout>


	// 同时还可以进行一些事件监听
	SwipeLayout swipeLayout =  (SwipeLayout)findViewById(R.id.sample1);

	//set show mode.
	swipeLayout.setShowMode(SwipeLayout.ShowMode.LayDown);

	//add drag edge.(If the BottomView has 'layout_gravity' attribute, this line is unnecessary)
	swipeLayout.addDrag(SwipeLayout.DragEdge.Left, findViewById(R.id.bottom_wrapper));

	swipeLayout.addSwipeListener(new SwipeLayout.SwipeListener() {
		    @Override
		    public void onClose(SwipeLayout layout) {
			//when the SurfaceView totally cover the BottomView.
		    }

		    @Override
		    public void onUpdate(SwipeLayout layout, int leftOffset, int topOffset) {
		       //you are swiping.
		    }

		    @Override
		    public void onStartOpen(SwipeLayout layout) {

		    }

		    @Override
		    public void onOpen(SwipeLayout layout) {
		       //when the BottomView totally show.
		    }

		    @Override
		    public void onStartClose(SwipeLayout layout) {

		    }

		    @Override
		    public void onHandRelease(SwipeLayout layout, float xvel, float yvel) {
		       //when user's hand released.
		    }
	});

