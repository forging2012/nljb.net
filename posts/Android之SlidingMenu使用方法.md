---
title: Android之SlidingMenu使用方法
date: '2015-03-17'
description:
categories:

tags:android

---

>

###  SlidingMenu 开源项目之侧滑菜单

>

	// 下载 ZXing 项目源码
	https://github.com/jfeinstein10/SlidingMenu

	// 解压得到 SlidingMenu-master -> library 目录
	SlidingMenu-master.zip

	// 在你项目的根目录创建一个lib目录
	[PATH]
	[app]
	    [src]
	    [res]
	    [build]
	    build.gradle
	    ...
	[build]
	[gradle]
	[lib]
	    // 将 SlidingMenu-master -> library 重名名为 sliding
	    [sliding]
	    [src]
	    [res]
	    [build]
	    ... 
	build.gradle
	settings.gradle
	...

	// 修改 settings.gradle 
	include ':app', ':lib:sliding'

	// 修改 [app]/build.gradle
	dependencies {
	    // Library
	    compile project(':lib:sliding')
	}

	// 修改 [lib]/[sliding]/build.gradle
	...
	buildscript {
	    repositories {
		mavenCentral()
	    }
	    dependencies {
		classpath 'com.android.tools.build:gradle:0.4.+'
	    }
	}
	apply plugin: 'android-library'

	dependencies {
	    compile 'com.android.support:support-v4:13.0.0'
	}

	android {
	    compileSdkVersion 17
	    buildToolsVersion "21.1.2"

	    defaultConfig {
		minSdkVersion 7
		targetSdkVersion 17
	    }

	    sourceSets {
		main {
		    java.srcDirs = ['src']
		    resources.srcDirs = ['src']
		    aidl.srcDirs = ['src']
		    renderscript.srcDirs = ['src']
		    res.srcDirs = ['res']
		    assets.srcDirs = ['assets']

		    manifest.srcFile 'AndroidManifest.xml'
		}
	    }

	}

>

---

>

### SlidingMenu 使用 

>

	// 设置左滑菜单
	menu.setMode(SlidingMenu.LEFT); 
	// 设置滑动的屏幕范围，该设置为全屏区域都可以滑动
	menu.setTouchModeAbove(SlidingMenu.TOUCHMODE_FULLSCREEN); 
	// 设置阴影图片
	menu.setShadowDrawable(R.drawable.shadow);
	// 设置阴影图片的宽度
	menu.setShadowWidthRes(R.dimen.shadow_width); 
	// SlidingMenu划出时主页面显示的剩余宽度
	menu.setBehindOffsetRes(R.dimen.slidingmenu_offset); 
	// 设置SlidingMenu菜单的宽度
	menu.setBehindWidth(400);
	// SlidingMenu滑动时的渐变程度
	menu.setFadeDegree(0.35f); 
	// 使SlidingMenu附加在Activity上
	menu.attachToActivity(this, SlidingMenu.SLIDING_CONTENT); 
	// 设置menu的布局文件
	menu.setMenu(R.layout.menu_layout); 
	// 动态判断自动关闭或开启SlidingMenu
	menu.toggle(); 
	// 显示SlidingMenu
	menu.showMenu(); 
	// 显示内容
	menu.showContent(); 
	// 监听slidingmenu打开
	menu.setOnOpenListener(onOpenListener); 

	// 监听slidingmenu关闭时事件
	menu.OnClosedListener(OnClosedListener); 
	// 监听slidingmenu关闭后事件
	menu.OnClosedListener(OnClosedListener); 

	// 属性，然后设置右侧菜单的布局文件
	menu.setMode(SlidingMenu.LEFT_RIGHT); 
	// 右侧菜单的阴影图片
	menu.setSecondaryShadowDrawable(R.drawable.shadowright); 


	// sliding_main.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent"
	    android:background="#ff000000">

	    <com.jeremyfeinstein.slidingmenu.lib.SlidingMenu
		android:id="@+id/sliding"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent">
		</com.jeremyfeinstein.slidingmenu.lib.SlidingMenu>

	</LinearLayout>
		
	// Activity
	public class MainActivity extends Activity {

	    SlidingMenu slidingMenu;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 创建 SlidingMenu
		slidingMenu = new SlidingMenu(MainActivity.this);
		// 附加 SlidingMenu 到 MainActivity
		// SLIDING_WINDOW会在SlidingMenu的内容部分包含ActionBar，而SLIDING_CONTENT不会
		slidingMenu.attachToActivity(MainActivity.this, SlidingMenu.SLIDING_CONTENT);
		// 设置左滑菜单
		slidingMenu.setMode(SlidingMenu.LEFT);
		//设置滑动的屏幕范围，该设置为全屏区域都可以滑动
		slidingMenu.setTouchModeAbove(SlidingMenu.TOUCHMODE_FULLSCREEN);
		//设置 menu 的布局文件
		slidingMenu.setMenu(R.layout.sliding_main);

	    }
	}


