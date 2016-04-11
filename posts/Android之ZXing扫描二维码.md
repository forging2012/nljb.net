---
title: Android之ZXing扫描二维码
date: '2015-03-17'
description:
categories:

tags:android

---

>

### 安装 ZXing 环境

>

	// 下载 ZXing 项目源码
	http://www.jikexueyuan.com/course/134_2.html

	// 解压得到 ZXingBarCode 目录
	ZXingBarCode.zip
	
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
	    // 将 ZXingBarCode 重名名为 barcode
	    [barcode]
		[src]
		[res]
		[build]
		... 
	build.gradle
	settings.gradle
	...

	// 修改 settings.gradle 
	include ':app', ':lib:barcode'

	// 修改 [app]/build.gradle
	dependencies {
	    // Library
	    compile project(':lib:barcode')
	}

	// 创建 [lib]/[barcode]/build.gradle
	apply plugin: 'android-library'
	android {
	    compileSdkVersion 17
	    buildToolsVersion "21.1.2"
	    sourceSets {
	    main {
		manifest.srcFile 'AndroidManifest.xml'
		java.srcDirs = ['src']
		resources.srcDirs = ['src']
		aidl.srcDirs = ['aidl']
		renderscript.srcDirs = ['src']
		res.srcDirs = ['res']
		assets.srcDirs = ['assets']
	    }
	    }
	}

>

---

>

### 遇到的一些问题

>

	// 权限不足问题
	W/FlashlightManager﹕ Unexpected error while invoking 
	public void android.os.IHardwareService$Stub$Proxy.setFlashlightEnabled(boolean) 
	throws android.os.RemoteException
	java.lang.SecurityException: Requires FLASHLIGHT or HARDWARE_TEST permission

	// 在 AndroidManifest.xml 增加一项
	<uses-permission android:name="android.permission.FLASHLIGHT" />

	// navbar.9.png 问题
	// 使用 draw9patch 编辑 navbar.9.png 增加边界坐标

	// 找不到 com.google.zxing.*
	// 在项目 -> Settings -> 选择 barcode -> Dependencies -> + File dependency -> 选择 barcode/libs/zxing.jar 

	// 两个软件启动图标问题
	// 修改 [barcode]/AndroidManifest.xml 去掉 <application>...</application>

>

---

>

### 使用方法

>

	// 修改 AndroidManifest.xml
	<?xml version="1.0" encoding="utf-8"?>
	<manifest xmlns:android="http://schemas.android.com/apk/res/android"
	    package="com.example.nljb.nljb" >

	    <!-- zxing library -->
	    <uses-permission android:name="android.permission.VIBRATE" />
	    <uses-permission android:name="android.permission.CAMERA" />
	    <uses-permission android:name="android.permission.FLASHLIGHT" />
	    <uses-feature android:name="android.hardware.camera" />
	    <uses-feature android:name="android.hardware.camera.autofocus" />
	    <!-- zxing library -->

	    <application
		android:allowBackup="true"
		android:icon="@drawable/ic_launcher"
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
		<!-- zxing library -->
		<activity
		    android:configChanges="orientation|keyboardHidden"
		    android:name="com.zxing.activity.CaptureActivity"
		    android:screenOrientation="portrait"
		    android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
		    android:windowSoftInputMode="stateAlwaysHidden" >
		</activity>
		<!-- zxing library -->
	    </application>

	</manifest>

	// 在 Activity 使用 Intent 使用 ZXing
	public class MainActivity extends Activity {

	    // ...
	    Button button;

	    // 获取 zxing 返回结果
	    @Override
	    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		if (resultCode == RESULT_OK && requestCode == 0)
		{
		    String result = data.getExtras().getString("result");
		    button.setText(result);
		}
	    }

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 通过按钮触发 zxing Activity 摄像头扫描
		button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			Intent intent = new Intent(MainActivity.this, CaptureActivity.class);
			startActivityForResult(intent, 0);

		    }
		});
	}

>

---

>

### 生成二维码

>

	// ...
	public class MainActivity extends Activity {

	    // ...
	    Button button;

	    ImageView imageView;

	    Bitmap qrcode;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

	        Button button2 = (Button) findViewById(R.id.button2);

		imageView = (ImageView) findViewById(R.id.imageView);

		button2.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {

			try {
			    // 生成 EncodingHandler.createQRCode , 内容及大小
			    qrcode = EncodingHandler.createQRCode("www.baidu.com", 400);
			} catch (WriterException e) {
			    e.printStackTrace();
			}
			imageView.setImageBitmap(qrcode);
		    }
		});
	}


