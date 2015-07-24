---
title: Android之Service使用方法
date: '2015-03-13'
description:
categories:

tags:android

---

>

### Service 之 stratService()

>

	// 生命周期
	stratService()
	onCreate()
	onStartCommand()
	// Service 运行中 ... 被自己或者调用者停止
	// 运行中可以通过 new Thread() ...
	onDestroy()

>

	// 创建 Service 还需要添加 AndroidManifest.xml
	<service
		android:name=".MyService"
		android:enabled="true"
		android:exported="true" >
	</service>

	// 创建 Service
	public class MyService extends Service {

	    public MyService() {}

	    // 该方法返回一个 IBinder 对象，应用程序可以通过该对象与 Service 交互
	    @Override
	    public IBinder onBind(Intent intent) {
			return null;
	    }

	    // Service 被创建时回调该方法
	    @Override
	    public void onCreate() {
			super.onCreate();
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
	    }

	    // Service 被启动时回调该方法
	    @Override
	    public int onStartCommand(Intent intent, int flags, int startId) {
			return START_STICKY;
	    }

	    // Service 被关闭之前回调
	    @Override
	    public void onDestroy() {
			super.onDestroy();
	    }
	}

	// 在 Activity 中使用 Service
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			final Intent intent = new Intent(MainActivity.this, MyService.class);
			Button button = (Button) findViewById(R.id.button);
			button.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
				// 启动 Service
				// startService(intent);
				// 关闭 Service
				// stopService(intent);
				}
			});
		}
	}

---

>

### Service 之 bindService

>

	// 生命周期
	bindService()
	onCreate()
	onBind()
	// 客户绑定到 Service ... unBindService() 取消绑定
	onUnbind()
	onDestroy()

>

	// 创建 Service 还需要添加 AndroidManifest.xml
	<service
		android:name=".MyService"
		android:enabled="true"
		android:exported="true" >
	</service>

	// 创建 Service
	public class MyService extends Service {
	
	    // 线程停止标记
	    private boolean quit;

	    // 计数器
	    private int count;

	    // 数据对象
	    private MyBinder binder = new MyBinder();

	    // 构造
	    public MyService() {}

	    // 数据类
	    public class MyBinder extends Binder {
			public int getCount() {
				return count;
			}
	    }

	    // 该方法返回一个 IBinder 对象，应用程序可以通过该对象与 Service 交互
	    @Override
	    public IBinder onBind(Intent intent) {
			// 将数据对象返回
			return binder;
	    }

	    // Service 被创建时回调该方法
	    @Override
	    public void onCreate() {
			super.onCreate();
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
			// 启动一个线程，处理数据
			new Thread()
			{
				@Override
				public void run()
				{
				while(!quit)
				{
					try {
					Thread.sleep(1000);
					} catch (InterruptedException e) {
					e.printStackTrace();
					}
					count++;
				}
				}
			}.start();
	    }

	    // Service 被启动时回调该方法
	    @Override
	    public int onStartCommand(Intent intent, int flags, int startId) {
			return START_STICKY;
	    }

	    // Service 被关闭之前回调
	    @Override
	    public void onDestroy() {
			super.onDestroy();
			// 关闭时，结束线程
			this.quit = true;
	    }

	}

	// 在 Activity 中使用 Service
	public class MainActivity extends Activity {

	    // 用户存储 Service 中的数据对象
	    MyService.MyBinder binder;

	    // 与 Service 建立连接
	    private ServiceConnection conn = new ServiceConnection() {

			// 当 Activity 与 Service 连接时回调该方法
			@Override
			public void onServiceConnected(ComponentName name, IBinder service) {
				// 获取返回的 Service 数据对象
				binder = (MyService.MyBinder) service;
			}

			// 当 Activity 与 Service 断开时回调该方法
			@Override
			public void onServiceDisconnected(ComponentName name) {}

	    };

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			final Intent intent = new Intent();
			intent.setAction("org.crazyit.service.FIRST_SERVICE");
			Button button = (Button) findViewById(R.id.button);
			button.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
				// 绑定 Service
				bindService(intent, conn, Service.BIND_AUTO_CREATE);
				// 解除绑定 Service
				// unbindService(conn);

				}
			});
			Button button2 = (Button) findViewById(R.id.button2);
			button2.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
				Toast.makeText(getApplicationContext(), String.valueOf(binder.getCount()), Toast.LENGTH_SHORT).show();
				}
			});
	    }
	}

---

>

### Service 之 IntentService

>

	IntentService继承Service,并在其创建了工作线程,用来处理耗时操作

>

	// 添加 AndroidManifest.xml
	<service
	    android:name=".MyIntentService"
	    android:exported="false" >
	</service>
	
	// 创建 IntentService
	public class MyIntentService extends IntentService {

	    public MyIntentService() {
		super("MyIntentService");
	    }

	    // 这里的运行，会在独立的线程内完成
	    @Override
	    protected void onHandleIntent(Intent intent) {
			Log.i("into", "xxx");
	    }

	}


	// 在 Activity 中使用 Service
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			Button button = (Button) findViewById(R.id.button);
			button.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
				Intent intent = new Intent(MainActivity.this, MyIntentService.class);
				startService(intent);
				stopService(intent);
				}
			});
 	    }   
	}


