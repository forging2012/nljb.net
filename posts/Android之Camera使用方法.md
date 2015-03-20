---
title: Android之Camera使用方法
date: '2015-03-20'
description:
categories:

tags:android

---

>

### Camera 使用方法

>

	// AndroidManifest.xml
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
	    <uses-permission
		android:name="android.permission.CAMERA">
		</uses-permission>
	</manifest>


	// activity_main.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    tools:context=".MainActivity">

	    <SurfaceView
		android:id="@+id/camera"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:layout_alignParentTop="true"
		android:layout_alignParentStart="true"
		android:layout_above="@+id/button" />


	    <Button
		android:id="@+id/button"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:layout_gravity="center_horizontal|bottom"
		android:text="拍照"
		android:layout_alignParentBottom="true"
		android:layout_alignParentStart="true" />


	</RelativeLayout>


	// Activity
	public class MainActivity extends Activity {

		private SurfaceView cameraPreview;

		Camera camera;

		Camera.Parameters parameters;

		@Override
		protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// ...
		cameraPreview = (SurfaceView) findViewById(R.id.camera);
		cameraPreview.getHolder().addCallback(new SurfaceHolder.Callback() {
		    @Override
		    public void surfaceCreated(SurfaceHolder holder) {
			camera = Camera.open();
			try {
			    // 设置 SurfaceView
			    camera.setPreviewDisplay(cameraPreview.getHolder());
			    // 获取 Parameters
			    parameters = camera.getParameters();
			    // 闪光灯
			    parameters.setFlashMode(Camera.Parameters.FLASH_MODE_TORCH);
			    // 用于拍照的连续自动对焦模式
			    parameters.setFocusMode(Camera.Parameters.FOCUS_MODE_CONTINUOUS_PICTURE);
			    // 设置 Parameters
			    camera.setParameters(parameters);
			    // 预览
			    camera.startPreview();
			    // 自动对焦
			    camera.cancelAutoFocus();
			} catch (IOException e) {
			    e.printStackTrace();
			}
		    }

		    @Override
		    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {
		    }

		    @Override
		    public void surfaceDestroyed(SurfaceHolder holder) {
			// 停止预览
			camera.stopPreview();
			// 释放
			camera.release();
		    }
		});

		// ...
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			camera.takePicture(null, null, new Camera.PictureCallback() {
			    @Override
			    public void onPictureTaken(byte[] data, Camera camera) {
				File dir = Environment.getExternalStorageDirectory();
				File file = new File(dir, "image.jpg");
				try {
				    if (!file.exists()) { file.createNewFile(); }
				    FileOutputStream fos = new FileOutputStream(file);
				    fos.write(data);
				    fos.flush();
				    fos.close();
				} catch (java.io.IOException e) {
				    e.printStackTrace();
				}
			    }
			});
		    }
		});
	}


