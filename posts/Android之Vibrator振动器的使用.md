---
title: Android之Vibrator振动器的使用
date: '2015-03-25'
description:
categories:

tags:android

---

>

### Vibrator

>

Android手机中的震动由Vibrator实现。

设置震动事件，需要知道其震动的时间长短、震动的周期等。

在Android Vibrator中，震动的时间一毫秒计算（1/1000秒）

>

	// AndroidManifest.xml
	<uses-permission android:name="android.permission.VIBRATE"></uses-permission>

	// Activity 控制振动器
	public class MainActivity extends Activity {

	    // 振动器
	    Vibrator vibrator;

	    @Override
	    public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 获取震动服务
		vibrator = (Vibrator) getSystemService(Service.VIBRATOR_SERVICE);
		Button button = (Button) findViewById(R.id.start);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			// 震动一秒
			vibrator.vibrate(1000);
			Intent intent;
			intent = new Intent(MainActivity.this, MainActivity2Activity.class);
			startActivity(intent);
		    }
		});
	    }
	}


