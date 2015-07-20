---
title: Android之SlidingDrawer的使用方法
date: '2015-07-14'
description:
categories:

tags:android

---

>

### SlidingDrawer

>

SlidingDrawer是自SDK 1.5才新加入的，实现Launcher的抽屉效果。

>

SlidingDrawer配置上采用了水平展开或垂直展开两种（android:orientation）方式

>

在XML里必须指定其使用的android:handle与android:content，前者委托要展开的图片（Layout配置），后者则是要展开的Layout Content。

>

<img src="{{urls.media}}/Android之SlidingDrawer的使用方法/1.gif" alt="" width="300" hight="480" >
<img src="{{urls.media}}/Android之SlidingDrawer的使用方法/2.gif" alt="" width="300" hight="480" >

>

---

>

	android:allowSingleTap	 	Indicates whether the drawer can be opened/closed by a single tap on the handle. 
	android:animateOnClick	 	Indicates whether the drawer should be opened/closed with an animation when the user clicks the handle. 
	android:bottomOffset	 	Extra offset for the handle at the bottom of the SlidingDrawer. 
	android:content	 			Identifier for the child that represents the drawer's content. 
	android:handle	 			Identifier for the child that represents the drawer's handle. 
	android:orientation	 		Orientation of the SlidingDrawer. 
	android:topOffset	 		Extra offset for the handle at the top of the SlidingDrawer. 

>

---

>

	<?xml version="1.0" encoding="utf-8"?>
	<SlidingDrawer xmlns:android="http://schemas.android.com/apk/res/android"
		android:id="@+id/drawer"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:content="@+id/content"
		android:handle="@+id/handle">

		<ImageView
			android:id="@id/handle"
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:src="@mipmap/ic_launcher"></ImageView>

		<ListView
			android:id="@id/content"
			android:layout_width="fill_parent"
			android:layout_height="fill_parent"
			android:background="#ff00ff"></ListView>

	</SlidingDrawer>  

>

---

>

	import android.app.Activity;  
	import android.os.Bundle;  
	import android.view.View;  
	import android.widget.AdapterView;  
	import android.widget.AdapterView.OnItemClickListener;  
	import android.widget.ArrayAdapter;  
	import android.widget.ImageView;  
	import android.widget.ListView;  
	import android.widget.SlidingDrawer;  
	import android.widget.SlidingDrawer.OnDrawerCloseListener;  
	import android.widget.SlidingDrawer.OnDrawerOpenListener;  
	import android.widget.Toast;  
	  
	public class SlidingDemo extends Activity implements OnItemClickListener,  
			OnDrawerOpenListener, OnDrawerCloseListener {  
		private SlidingDrawer drawer;  
		private ImageView handle;  
		private ListView content;  
	  
		@Override  
		public void onCreate(Bundle savedInstanceState) {  
			super.onCreate(savedInstanceState);  
			setContentView(R.layout.sliding);  
	  
			drawer = (SlidingDrawer) this.findViewById(R.id.drawer);  
			handle = (ImageView) this.findViewById(R.id.handle);  
			content = (ListView) this.findViewById(R.id.content);  
			content.setAdapter(new ArrayAdapter<String>(this,  
					android.R.layout.simple_list_item_1, new String[] { "one",  
							"two", "three" }));  
			content.setOnItemClickListener(this);  
	  
			// 设置SlidingDrawer打开或者关闭时的监听器  
			drawer.setOnDrawerOpenListener(this);  
			drawer.setOnDrawerCloseListener(this);  
		}  
	  
		@Override  
		public void onItemClick(AdapterView<?> parent, View view, int position,  
				long id) {  
			Toast.makeText(this, "you clicked position " + (position + 1),  
					Toast.LENGTH_SHORT).show();  
		}  
	  
		// SlidingDrawer关闭时回调  
		@Override  
		public void onDrawerClosed() {  
			handle.setImageResource(R.drawable.up);  
		}  
	  
		// SlidingDrawer打开时回调  
		@Override  
		public void onDrawerOpened() {  
			handle.setImageResource(R.drawable.down);  
		}  
	  
	}

>

---

>

	void	setOnDrawerCloseListener(SlidingDrawer.OnDrawerCloseListener onDrawerCloseListener)
		Sets the listener that receives a notification when the drawer becomes close.
	void	setOnDrawerOpenListener(SlidingDrawer.OnDrawerOpenListener onDrawerOpenListener)
		Sets the listener that receives a notification when the drawer becomes open.  

>

	转载来自：http://blog.csdn.net/moreevan/article/details/6741083 （感谢）
