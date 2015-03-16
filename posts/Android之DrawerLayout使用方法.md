---
title: Android之DrawerLayout使用方法
date: '2015-03-13'
description:
categories:

tags:android

---

	// 定义一个 android.support.v4.widget.DrawerLayout 
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent" >

	    <android.support.v4.widget.DrawerLayout
		android:id="@+id/drawer_layout"
		android:layout_width="match_parent"
		android:layout_height="match_parent">

		<!-- The main content view -->

		<FrameLayout
		    android:id="@+id/content_frame"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent" >

		    <Button
			android:id="@+id/btn"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:text="open"
			/>
		</FrameLayout>

		<!-- The navigation drawer -->

		<ListView
		    android:id="@+id/left_drawer"
		    android:layout_width="240dp"
		    android:layout_height="match_parent"
		    android:layout_gravity="start"
		    android:background="#111"
		    android:choiceMode="singleChoice"
		    android:divider="@android:color/transparent"
		    android:dividerHeight="0dp" />
	    </android.support.v4.widget.DrawerLayout>

	</RelativeLayout>

	// Activity
	public class MainActivity extends Activity
	{
		private DrawerLayout mDrawerLayout = null;

		@Override
		protected void onCreate(Bundle savedInstanceState)
		{
		    super.onCreate(savedInstanceState);
		    setContentView(R.layout.a);

		    mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);

		    Button button = (Button) findViewById(R.id.btn);
		    button.setOnClickListener(new View.OnClickListener()
		    {

			@Override
			public void onClick(View v)
			{
			    // 按钮按下，将抽屉打开
			    mDrawerLayout.openDrawer(Gravity.LEFT);

			}
		    });
		}

	}


