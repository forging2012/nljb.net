---
title: Android之Activity间数据传递
date: '2015-03-09'
description:
categories:

tags:android

---

### Activity间数据传递

>

---

>

### Activity 生命周期介绍

>

	活动状态，当前Activity位于前台，用户可见，可以获得焦点
	暂停状态，其它Activity位于前台，该Activity依然可见，只是不能获得焦点
	停止状态，该Activity不可见，失去焦点
	销毁状态，该Activity结束，或Activity所在的Dalvik进程被结束

	// 创建 Activity 时被回调，该方法只会被调用一次
	onCreate(Bundle savedStatus)
	// 启动 Activity 时被回调
	onStart()
	// 重新启动 Activity 时被回调
	onRestart()
	// 恢复 Activity 时被回调，onStart() 方法后一定会回调 onResume() 方法
	onResume()
	// 暂停 Activity 时被回调
	onPause()
	// 停止 Activity 时被回调
	onStop()
	// 销毁 Activity 时被回调，该方法只会被调用一次
	onDestroy()

	@Override
	public void onPause()
	{
	    super.onPause();
	}

	@Override
	public void onStart()
	{
	    super.onStart();
	}

	...

>

---

>

### Result 的相关介绍

>

	// 启动一个 Activity 并且设置它的 requestCode
	startActivityForResult -> requestCode

	// 在 Activity 关闭前设置一个返回状态 resultCode
	setResult -> resultCode

	// 监听 Activity 的返回的 requestCode 和 resultCode 与 intent
	onActivityResult -> requestCode -> resultCode -> intent

>

---

>

### 补充关于传递一个类

>

	public class User implements Serializable {
		 private String username;
	}

	User user = new User();
	user.username = "root";

	Intent intent = new Intent(LoginActivity.this, MyNavigationDrawer.class);
	intent.putExtra("USER_INFO", (Serializable) user);
	startActivity(intent);

	Intent intent = getIntent();
	User user = (User) intent.getSerializableExtra("USER_INFO");
    Log.i("INFO", user.getUsername());

>

---

>

### 将数据传递到下一个，Activity

>

	// 发送端的 Activity
	public class MainActivity extends Activity {

	    View root;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			send();
		    }
		});
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
			/*
				这里简单介绍一下
				创建一个AlertDialog对话框
				创建一个，确定按钮，并且监听事件
				点击按钮时，将一个对象（可以是一个obj或int,string,bool等)传递出去
			*/
			EditText edit = (EditText) root.findViewById(R.id.editText);
			EditText edit2 = (EditText) root.findViewById(R.id.editText2);
			Bundle data = new Bundle();
			data.putString("username", edit.getText().toString());
			data.putString("password", edit2.getText().toString());
			/*
			    // 同样，Bundle 也可以传递一个类，比如
			    Person person = new Person("name", "pass" ,"...");
			    data.putSerializable("person", person);
			    // 取出时这样就可以
			    Intent intent = getIntent();
			    Person person = (Person) intent.getSerializableExtra("person");
			 */
			Intent intent = new Intent(MainActivity.this, BundleActivity.class);
			intent.putExtras(data);
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


	// 接收端
	public class BundleActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		/*
			通过 getIntent() 获取发送的 intent
			就可以通过，intent 取值了
		*/
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_bundle);
		TextView text = (TextView) findViewById(R.id.xxx);
		Intent intent = getIntent();
		text.setText(intent.getStringExtra("username"));
	    }

	}

---

>

### 将数据返回到上一个 Activit

>

	// 这里负责启动一个 Activit 并接收其返回的数据
	public class MainActivity extends Activity {

	    View root;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			send();
		    }
		});
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
		// 添加一个按钮，并监听按钮事件 (消极的;否认的)
		builder.setNegativeButton("取消", new DialogInterface.OnClickListener() {
		    @Override
		    public void onClick(DialogInterface dialog, int which) {
			// 创建 Intent
			Intent intent = new Intent(MainActivity.this, ControlActivity.class);
			// 以需要返回数据的模式启动该 Activity
			// 并传递(请求码)
			startActivityForResult(intent, 0);
		    }
		});
		// 可以设置 View
		builder.setView(root);
		// 创建及显示
		builder.create().show();
		// .......
	    }

	    // 上一个 Activity 返回时触发此方法
	    @Override
	    public void onActivityResult(int requestCode, int resultCode, Intent intent)
	    {
		// 判断请求码与返回码
		if (requestCode == 0 && resultCode == 0)
		{
		    // 获取 Bundle
		    Bundle data = intent.getExtras();
		    // 通过 Key 获取 Value
		    String resultCity = data.getString("city");
		    // 获取 View 对象
		    TextView text = (TextView) findViewById(R.id.show);
		    // 设置对象值
		    text.setText(resultCity);
		}
	    }

	}

	
	// 这里就是被启动的 Activit , 并且在退出时返回数据
	public class ControlActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_control);
		// 获取 Intent
		Intent intent = getIntent();
		// 保存需要返回的数据
		// 当然，返回可以是一个（类，或者 int, string, bool ...)
		intent.putExtra("city", "Google");
		// 返回，并且设置(返回码)
		this.setResult(0, intent);
		// 关闭
		this.finish();
	    }

	}


