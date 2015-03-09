---
title: Android之GestureDetector手势检测
date: '2015-03-07'
description:
categories:

tags:android

---

### GestureDetector 实例代表了一个手势检测器

>

也就是,在Activity中将onTouchEvent事件交给GestureDetector检测

>

---

	// 这里注意，要 implements GestureDetector.OnGestureListener 来实现一些动作方法
	public class MainActivity extends Activity implements GestureDetector.OnGestureListener {

	    View root;

	    GestureDetector detector;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		detector = new GestureDetector(this, (GestureDetector.OnGestureListener) this);
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			send();
		    }
		});
	    }

	    // 这里，一定要把 onTouchEvent 交给 detector.onTouchEvent 
	    @Override
	    public boolean onTouchEvent(MotionEvent me)
	    {
		return detector.onTouchEvent(me);
	    }

	    // 当触碰事件按下时触发该方法
	    @Override
	    public boolean onDown(MotionEvent e) {
		Log.i("info", "onDown");
		return false;
	    }

	    // 当用户在触摸屏上按下，而且还未移动和松开时触发该方法
	    @Override
	    public void onShowPress(MotionEvent e) {
		Log.i("info", "onShowPress");
	    }

	    // 用户在触摸屏上的轻击事件将会触发该方法
	    @Override
	    public boolean onSingleTapUp(MotionEvent e) {
		Log.i("info", "onSingleTapUp");
		return false;
	    }

	    // 当初始按下动作事件和当前移动动作事件倒置滚动时通知本方法
	    @Override
	    public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY) {
		Log.i("info", "onScroll");
		return false;
	    }

	    // 初始化并按下触发长安并通知本方法
	    @Override
	    public void onLongPress(MotionEvent e) {
		Log.i("info", "onLongPress");
	    }

	    // 屏幕手势滑动中
	    @Override
	    public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {
		Log.i("info", "onFling");
		// Y 方向
		if (e2.getY() - e1.getY() > 50)  // 从上向下滑动
		{
		    // ...
		} else if (e1.getY() - e2.getY() > 50) // 从下向上滑动
		{
		    // ...
		}
		// X 方向
		if (e1.getX() - e2.getX() > 50) // 从右向左滑动
		{
		    // ...
		} else if (e2.getX() - e1.getX() > 50) // 从左向右滑动
		{
		    // ...
		}
		return false;
	    }

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

---
...
