---
title: Android之多点触摸坐标获取
date: '2015-05-05'
description:
categories:

tags:android

---

>

### 多点触摸坐标

>

	// activity_main.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/activity_main"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical"
	    tools:context=".MainActivity"
	    tools:ignore="MergeRootFrame">
	</RelativeLayout>

	// MainActivity
	public class MainActivity extends Activity {

	    RelativeLayout layout = null;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		layout = (RelativeLayout) findViewById(R.id.activity_main);
		layout.setOnTouchListener(new View.OnTouchListener() {
		    @Override
		    public boolean onTouch(View v, MotionEvent event) {
			switch (event.getAction()) {
			    case MotionEvent.ACTION_DOWN: // 点击
				break;
			    case MotionEvent.ACTION_MOVE: // 移动
				// 触摸点数量
				// event.getPointerCount();
				// 触摸点坐标 0
				// event.getX(0); event.getY(0);
				// 触摸点坐标 1
				// event.getX(1); event.getY(1);
				// ...
				break;
			    case MotionEvent.ACTION_UP: // 抬起
				break;
			}
			return true;
		    }
		});
	    }
	}




