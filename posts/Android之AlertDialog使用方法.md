---
title: Android之AlertDialog使用方法
date: '2015-03-07'
description:
categories:

tags:android

---

### 什么是AlertDialog?什么是AlertDialog.Builder?

>

AlertDialog是Dialog的一个直接子类

一个AlertDialog可以有两个Button或者3个Button	

可以对一个AlertDialog设置title、message。

不能直接通过AlertDialog的构造函数来生成一个AlertDialog

一般生成的时候都是通过它的的一个内部静态类AlertDialog.Builder来构造的。

---

	public void send()
	{
	// ....... 构造一个 AlertDialog
	AlertDialog.Builder builder = new AlertDialog.Builder(this)
		// 对话框标题
		.setTitle("简单对话框")
		// 对话框图标
		.setIcon(R.mipmap.ic_launcher);
		// 对话框内容
		// .setMessage("对话框内容.");
	// 添加一个按钮，并监听按钮事件 (积极的;确实的)
	builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which) {
		Intent intent = new Intent(MainActivity.this, ControlActivity.class);
		startActivity(intent);
	    }
	});
	// 添加一个按钮，并监听按钮事件 (消极的;否认的)
	builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which) {
		Intent intent = new Intent(MainActivity.this, ControlActivity.class);
		startActivity(intent);
	    }
	});
	// 添加一个按钮，并监听按钮事件 (中立的)
	builder.setNeutralButton("帮助", new DialogInterface.OnClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which) {
		Intent intent = new Intent(MainActivity.this, ControlActivity.class);
		startActivity(intent);
	    }
	});
	// 列表
	CharSequence chars[] = {"hello", "wrold"};
	// 列表项目对话框
	builder.setItems(chars, new DialogInterface.OnClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which) {
		// 点击了第几列
		Log.i("info", String.valueOf(which));
		// ... 动作
		Intent intent = new Intent(MainActivity.this, ControlActivity.class);
		startActivity(intent);
	    }
	});
	// 单选按钮
	builder.setSingleChoiceItems(chars, 0, new DialogInterface.OnClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which) {
		// 点击了第几列
		Log.i("info", String.valueOf(which));
		// ... 动作
		Intent intent = new Intent(MainActivity.this, ControlActivity.class);
		startActivity(intent);
	    }
	});
	boolean m_boolean[] = {false, false};
	// 多选按钮
	builder.setMultiChoiceItems(chars, m_boolean, new DialogInterface.OnMultiChoiceClickListener() {
	    @Override
	    public void onClick(DialogInterface dialog, int which, boolean isChecked) {
		// 获取点击事件，并进行记录
		Log.i("info", String.valueOf(which) + " " + String.valueOf(isChecked));
	    }
	});
	// 获取一个 View
	// View root = this.getLayoutInflater().inflate(R.layout.activity_control, null);
	// 可以设置 View
	// builder.setView(root);
	// 创建及显示
	builder.create().show();
	// .......
	}

---

### 自定义 AlertDialog

>

	public class MainActivity extends Activity {

	    // 全局 View 
	    View root;

	    public void send()
	    {
		// 获取一个 View
		root = this.getLayoutInflater().inflate(R.layout.activity_control, null);
		// .......
		AlertDialog.Builder builder = new AlertDialog.Builder(this)
			// 对话框标题
			.setTitle("简单对话框")
			// 对话框图标
			.setIcon(R.mipmap.ic_launcher);
			// 对话框内容
			// .setMessage("对话框内容.");
		// 添加一个按钮，并监听按钮事件 (积极的;确实的)
		builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
		    @Override
		    public void onClick(DialogInterface dialog, int which) {
			// 获取结果
			EditText edit = (EditText) root.findViewById(R.id.editText);
			EditText edit2 = (EditText) root.findViewById(R.id.editText2);
			Log.i("info", edit.getText().toString());
			Log.i("info", edit2.getText().toString());
		    }
		});
		// 添加一个按钮，并监听按钮事件 (消极的;否认的)
		builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
		    @Override
		    public void onClick(DialogInterface dialog, int which) {
			Intent intent = new Intent(MainActivity.this, ControlActivity.class);
			startActivity(intent);
		    }
		});
		// 可以设置 View
		builder.setView(root);
		// 创建及显示
		builder.create().show();
		// .......
	    }

	}


>

	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
	    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    tools:context="com.example.nljb.nljb.ControlActivity">

	    <LinearLayout
		android:orientation="vertical"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:layout_centerHorizontal="true"
		android:layout_alignParentTop="true">

		<LinearLayout
		    android:orientation="horizontal"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:gravity="center">

		    <TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="用户名"
			android:id="@+id/textView" />

		    <EditText
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:inputType="textPersonName"
			android:text="Name"
			android:ems="10"
			android:id="@+id/editText" />
		</LinearLayout>

		<LinearLayout
		    android:orientation="horizontal"
		    android:layout_width="fill_parent"
		    android:layout_height="fill_parent"
		    android:gravity="center">

		    <TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="密    码"
			android:id="@+id/textView2" />

		    <EditText
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:inputType="numberPassword"
			android:ems="10"
			android:id="@+id/editText2" />
		</LinearLayout>

	    </LinearLayout>
	</RelativeLayout>

