---
title: Android之Fragment使用方法
date: '2015-03-11'
description:
categories:

tags:android

---

>

### 先介绍 FrameLayout

>

	// 主要有三点
	1，有个主(FrameLayout)供你添加或替换(Fragment)
	2，有几个(Fragment)供你添加到(FrameLayout)
	3，有个事件触发(Fragment)间的添加或者替换(Fragment)

	// Fragment生命周期的核心函数
	// 从fragment运行起来到恢复状态(跟用户紧密相关的)有：
	onAttach(Activity) 
	// 当Fragment关联activity时调用onAttach（Activity）。
	onCreate(Bundle) 
	// 第一次创建Fragment的时候调用onCreate（Bundle）。
	onCreateView(LayoutInflater, ViewGroup, Bundle) 
	// 创建一个跟Fragment关联的视图层级。
	onActivityCreated(Bundle) 
	// 函数告诉Fragment跟它关联的activity已经完成Activity.onCreaate。
	onStart() 
	// 函数可以让用户看见Fragment（条件是Activity已经启动）。
	onResume() 
	// 使Fragment跟用户交互（条件是Activity已经恢复了）
	// 当一个Fragment不再使用，它必须经历一系列反向回调：
	onPause() 
	// 不论因为Activity被暂停或者Fragment的操作正在修改导致Fragment不再和用户交互。
	onStop() 
	// 不论因为Activity已经停止或者Fragment的操作正在修改导致用户不能看见Fragment。
	onDestroyView() 
	// Fragment清理和它关联的View的资源。
	onDestroy()
	// 清理Fragment的状态。
	onDetach()
	// 在Fragment不再和Activity关联之前调用onDetach()。	

>

	// 一，主(FrameLayout)
	// activity_main.xml
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:id="@+id/container"
	    android:layout_width="match_parent" android:layout_height="match_parent"
	    tools:context=".MainActivity" tools:ignore="MergeRootFrame" >

	    // 这个主(FrameLayout)已经有个内置的(Fragment)了
	    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
		android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
		android:paddingRight="@dimen/activity_horizontal_margin"
		android:paddingTop="@dimen/activity_vertical_margin"
		android:paddingBottom="@dimen/activity_vertical_margin"
		tools:context=".MainActivity$PlaceholderFragment">

		<Button
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:text="New Button"
		    android:id="@+id/button"
		    android:layout_below="@+id/textView"
		    android:layout_alignParentLeft="true"
		    android:layout_marginTop="173dp" />

	    </RelativeLayout>

	</FrameLayout>

	// 二，主类
	// MainActivity.class
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 这里将一个(Fragment)添加到(FrameLayout)中
		if (savedInstanceState == null) {
		    getFragmentManager().beginTransaction()
			    // 添加
			    .add(R.id.container, new PlaceholderFragment())
			    .commit();
		}
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			// Bundle 存值
			Bundle bundle = new Bundle();
			bundle.putString("key", "value");
			// Fragment 对象
			Fragment fr = new Fragment1();
			// 保存
			fr.setArguments(bundle);
			// 这里将一个(Fragment)替换到(FrameLayout)中的(Fragment)
			getFragmentManager().beginTransaction()
				// 替换
				.replace(R.id.container, fr)
				.commit();
		    }
		});
	    }

	    // 创建了一个(Fragment)并返回(Fragment)的(xLayout)
	    public static class PlaceholderFragment extends Fragment {

		public PlaceholderFragment() {
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
					 Bundle savedInstanceState) {
		    View rootView = inflater.inflate(R.layout.fragment_main, container, false);
		    return rootView;
		}
	    }

	   // 创建了一个(Fragment)并返回(Fragment)的(Layout)
	   public static class Fragment1 extends Fragment {

		public Fragment1() {
		}

		String val;
		Button button;

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
					 Bundle savedInstanceState) {
		    // 获取传过来的值
		    val  = getArguments().getString("key");
		    Log.i("info", val);
		    // 初始化 View
		    View rootView = inflater.inflate(R.layout.fragment, container, false);
		    // 获取 Button
		    button = (Button) rootView.findViewById(R.id.button2);
		    // 监听 Button 事件
		    button.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
			    // 将值设置为按钮名
			    button.setText(val);
			}
		    });

		    return rootView;
		}
	    }

	// 三，两个(Layout)
	// fragment.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <Switch
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="New Switch"
		android:id="@+id/switch1" />

	    // 加个按钮，上面演示了如何使用该按钮
	    <Button
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="New Button"
		android:id="@+id/button2" />

	</LinearLayout>

	// fragment_main.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
	    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    tools:context=".MainActivity$PlaceholderFragment">

	    <TextView android:text="@string/hello_world" android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:id="@+id/textView" />

	</RelativeLayout>

