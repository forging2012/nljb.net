---
title: Android之Service使用方法
date: '2015-03-13'
description:
categories:

tags:android

---

>

### 在 Service 通知 Activity 更新 UI

>

解决方法：将 Hander 传入 Service 由 Service 发送更新消息到 Activity

>

	// 实例
	public class UpdateService extends Service {

		// ---------------------------------------

		private static android.os.Handler mHandler;

		// ---------------------------------------

		// 启动Service并传入Handler
		public static void onStart(Context context, android.os.Handler handler) {
			mHandler = handler;
			context.startService(new Intent(context, UpdateService.class));
		}

		// .... 随后就可以使用 mHandler 发送通知了 ...

	}

>

---

>

### 每次 startService 都会被 执行的 onStartCommand

>

	在 Service 后台运行时，如果程序退出后需要再启动
		这时 Service 的 onCreate 是不会执行的.
		每次 startService 时 onStartCommand 都会被执行 ...

>

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

---

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

	当一个Service程序启动后，如果没有出现意外且明确调用stopService()方法
		则它将会一直驻留在手机的服务之中。

	如果希望由Activity启动的Service程序可以在Activity程序结束后自动结束
		那么就应该将Activity和Service程序进行绑定。

>

	为此需要借助android.content.ServiceConnection接口
		此接口的主要功能是当一个Activity程序与Service建立连接之后
		执行Service连接或取消连接的处理操作

	在Activity连接到Service程序之后，会触发Service类中的onBind()方法
		在此方法中要返回一个android.os.IBinder接口的对象。

	默认情况下，当一个Activity程序启动Service之后，该Service程序就会在后台独自运行
		与前台的Activity不再有什么联系，但是如果使用ServiceConnection进行连接
		则这个Service就会和相应的Activity程序绑定在一起，而不再是独立运行了。

	// 当与一个Service建立连接时调用
	public abstract void onServiceConnected(ComponenetName name, IBinderservice)
	// 当与一个Service取消连接时调用
	public abstract void onServiceDisconnected(ComponentName name)

---

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


