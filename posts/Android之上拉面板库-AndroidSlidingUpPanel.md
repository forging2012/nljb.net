---
title: Android之上拉面板库-AndroidSlidingUpPanel
date: '2015-06-15'
description:
categories:

tags:android

---

<img src="{{urls.media}}/Android之上拉面板库-AndroidSlidingUpPanel/show.png" alt="" width="700" hight="300" >

---

	https://github.com/umano/AndroidSlidingUpPanel

---

	dependencies {
	    repositories {
		mavenCentral()
	    }
	    compile 'com.sothree.slidinguppanel:library:3.0.0'
	}

---

	SlidingUpPanelLayout slidingUpPanelLayout = (SlidingUpPanelLayout) findViewById(R.id.sliding_layout);
	slidingUpPanelLayout.setPanelSlideListener(new SlidingUpPanelLayout.PanelSlideListener() {
	    @Override
	    public void onPanelSlide(View view, float v) {

	    }

	    @Override
	    public void onPanelCollapsed(View view) {
		// 向下移动，图标变成向下
		ImageView imageView = (ImageView) findViewById(R.id.menuUpward);
		imageView.setImageResource(R.drawable.directional_up);

	    }

	    @Override
	    public void onPanelExpanded(View view) {
		// 向下移动，图标变成向上
		ImageView imageView = (ImageView) findViewById(R.id.menuUpward);
		imageView.setImageResource(R.drawable.directional_down);
	    }

	    @Override
	    public void onPanelAnchored(View view) {
	    }

	    @Override
	    public void onPanelHidden(View view) {
	    }
	});

---

	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:custom="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical">

	    <LinearLayout
		android:layout_width="match_parent"
		android:layout_height="0dp"
		android:layout_weight="1">

		<com.sothree.slidinguppanel.SlidingUpPanelLayout xmlns:sothree="http://schemas.android.com/apk/res-auto"
		    android:id="@+id/sliding_layout"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent"
		    android:gravity="bottom"
		    sothree:umanoDragView="@+id/dragView"
		    sothree:umanoOverlay="true"
		    sothree:umanoPanelHeight="68dp"
		    sothree:umanoParalaxOffset="100dp"
		    sothree:umanoShadowHeight="4dp">

		    <!-- MAIN CONTENT -->
		    <LinearLayout
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:layout_weight="1"
			android:background="#2b6cb8"
			android:orientation="vertical">

			<com.github.lzyzsd.circleprogress.ArcProgress
			    android:id="@+id/arc_progress"
			    android:layout_width="200dp"
			    android:layout_height="200dp"
			    android:layout_centerHorizontal="true"
			    android:layout_centerVertical="true"
			    android:background="#00ffffff"
			    custom:arc_bottom_text="德纳科技"
			    custom:arc_finished_color="#FFFFFFFF"
			    custom:arc_progress="0"
			    custom:arc_text_color="#FFFFFFFF"
			    custom:arc_unfinished_color="#5576B6"
			    android:layout_gravity="center_horizontal"
			    android:layout_margin="50dp" />

			<ListView
			    android:id="@+id/list_keys"
			    android:background="#ffffff"
			    android:layout_width="match_parent"
			    android:layout_height="match_parent"
			    android:layout_gravity="center_horizontal|top"></ListView>

		    </LinearLayout>

		    <!-- SLIDING LAYOUT -->
		    <LinearLayout
			android:id="@+id/dragView"
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:background="#ffffff"
			android:clickable="true"
			android:focusable="false"
			android:orientation="vertical">

			<TextView
			    android:layout_width="match_parent"
			    android:layout_height="68dp"
			    android:background="@drawable/menu_mask"
			    android:gravity="center"
			    android:text="正在检查 ..."
			    android:textSize="16sp" />

			<ListView
			    android:id="@+id/list_values"
			    android:layout_width="match_parent"
			    android:layout_height="match_parent"></ListView>

		    </LinearLayout>
		</com.sothree.slidinguppanel.SlidingUpPanelLayout>
	    </LinearLayout>

	</LinearLayout>

