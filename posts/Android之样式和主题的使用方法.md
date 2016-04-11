---
title: Android之样式和主题的使用方法
date: '2015-03-20'
description:
categories:

tags:android

---

>

### 样式和主题

>

Android style 可以配置给各种布局对象, 进行分组配置

>
	
	// 样式文件 styles.xml
	<resources>

	    <!-- Base application theme. -->
	    <style name="AppTheme" parent="android:Theme.Holo.Light.DarkActionBar">
		<!-- Customize your theme here. -->
	    </style>
	    
	    <style name="MyTheme">
		<item name="android:text">aaa</item>
		<item name="android:textSize">18pt</item>
	    </style>

	    <style name="MyView">
		<item name="android:background">#ff0</item>
	    </style>

	</resources>

	// 配置给 Button
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent"
	    android:orientation="vertical">

	    <Button
	    android:id="@+id/a"
		style="@style/MyTheme"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"/>

	    <Button
		style="@style/MyTheme"
		android:id="@+id/b"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"/>
	...
	
	// 配置给 Layout
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
	    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity"
	    android:orientation="vertical"
	    style="@style/MyView">
	...

	// 配置给 APP
	<?xml version="1.0" encoding="utf-8"?>
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	    package="com.example.nljb.nljb" >

	    <application
		android:allowBackup="true"
		android:icon="@mipmap/ic_launcher"
		android:label="@string/app_name"
		android:theme="@style/AppTheme" >
		<activity
		    android:name=".MainActivity"
		    android:label="@string/app_name" >
		    <intent-filter>
			<action android:name="android.intent.action.MAIN" />
			<category android:name="android.intent.category.LAUNCHER" />
		    </intent-filter>
		</activity>
	    </application>

	    <uses-permission
		android:name="android.permission.WRITE_EXTERNAL_STORAGE">
	    </uses-permission>

	</manifest>


