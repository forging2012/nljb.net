---
title: Android之闪烁发光字体效果
date: '2015-08-25'
description:
categories:

tags:android

---

>

### 闪烁发光字体

> 

Facebook开源了一款加载效果工具Shimmer，可以实现字体的闪闪发光效果，效果如下

>

	https://github.com/facebook/Shimmer，针对iOS开发实现

	http://facebook.github.io/shimmer-android/ 针对Android开发实现

	https://github.com/RomainPiel/Shimmer-android 针对Android开发实现

>

<img src="{{urls.media}}/Android之闪烁发光字体效果/1.gif" alt="" width="300" hight="160">

>

	<com.facebook.shimmer.ShimmerFrameLayout
		android:id="@+id/shimmer_view_container"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:layout_below="@+id/welcome_slogan"
		android:layout_centerHorizontal="true"
		android:layout_gravity="center"
		android:layout_marginTop="150dp"
		shimmer:duration="1000">

		<ImageButton
			android:id="@+id/welcome_start"
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:layout_alignParentBottom="true"
			android:layout_centerHorizontal="true"
			android:background="@drawable/welcome_start" />
	</com.facebook.shimmer.ShimmerFrameLayout>

>

	private ShimmerFrameLayout mShimmer = null;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_welcome);
		mShimmer = (ShimmerFrameLayout) findViewById(R.id.shimmer_view_container);
	}

	@Override
	public void onResume() {
		super.onResume();
		if (mShimmer != null) {
			mShimmer.startShimmerAnimation();
		}
	}

	@Override
	public void onPause() {
		if (mShimmer != null) {
			mShimmer.stopShimmerAnimation();
		}
		super.onPause();
	}

