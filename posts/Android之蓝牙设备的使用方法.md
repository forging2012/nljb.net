---
title: Android之蓝牙设备的使用方法
date: '2015-03-27'
description:
categories:

tags:android

---

	// 
	public class MainActivity extends Activity {

	    private BluetoothAdapter bluet;

	    private final static int REQUEST_ENABLE_BT = 1;

	    // Create a BroadcastReceiver for ACTION_FOUND
	    private final BroadcastReceiver mReceiver = new BroadcastReceiver() {
		@Override
		public void onReceive(Context context, Intent intent) {
		    // 获取 Action
		    String action = intent.getAction();
		    // 查找到设备
		    if (BluetoothDevice.ACTION_FOUND.equals(action)) {
			// 蓝牙设备
			BluetoothDevice device = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE);
			Log.i("INFO", device.getName() + "," + device.getAddress());
			// 搜索完成
		    } else if (BluetoothAdapter.ACTION_DISCOVERY_FINISHED.equals(action)) {
			Toast.makeText(MainActivity.this, "搜索完毕", Toast.LENGTH_SHORT).show();
		    }
		}
	    };

	    @Override
	    public void onDestroy() {
		super.onDestroy();
		// 关闭服务查找
		if (bluet != null) {
		    bluet.cancelDiscovery();
		}
		// 注销接收器
		unregisterReceiver(mReceiver);
	    }

	    @Override
	    public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 获取蓝牙适配器 bluetoothadapter
		bluet = BluetoothAdapter.getDefaultAdapter();
		if (bluet == null) { // 是否支持

		} else if (!bluet.isEnabled()) { // 是否开启
		    // 开启蓝牙
		    Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
		    startActivityForResult(enableBtIntent, REQUEST_ENABLE_BT);
		}

		// 注册接收查找到设备action接收器
		IntentFilter filter = new IntentFilter(BluetoothDevice.ACTION_FOUND);
		registerReceiver(mReceiver, filter);

		// 注册查找结束action接收器
		filter = new IntentFilter(BluetoothAdapter.ACTION_DISCOVERY_FINISHED);
		registerReceiver(mReceiver, filter);

		// ...
		Button button = (Button) findViewById(R.id.start);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			// 关闭再进行的服务查找
			if (bluet.isDiscovering()) {
			    bluet.cancelDiscovery();
			}
			bluet.startDiscovery();
		    }
		});

		// ...
		Button stop = (Button) findViewById(R.id.stop);
		stop.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			bluet.cancelDiscovery();
		    }
		});
	    }

	}

