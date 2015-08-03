---
title: Android之百度地图坐标定位
date: '2015-08-03'
description:
categories:

tags:android

---

>

### 百度地图坐标定位

>

百度地图Android定位SDK提供GPS，基站，Wi-Fi等多种定位方式

>

适用于室、内外多种定位场景，具有出色的定位性能：

>

定位精度高、覆盖率广、网络定位请求流量小、定位速度快。

>

---

>

<img src="{{urls.media}}/Android之百度地图坐标定位/1.jpeg" alt="" width="350" hight="600" >

>

---

>

	1，在百度开放平台创建应用，并且设置安全码
		地址：http://lbsyun.baidu.com/apiconsole/key

	2，获取应用访问应用（AK）...

	3，从百度开放平台下载SDK
		地址：http://lbsyun.baidu.com/sdk/download

	4，基础SDK开发包：定位功能，基础地图，检索功能，LBS云检索
		获取文件：BaiduLBS_Android.jar + armeabi（目录）

	5，将armeabi目录内文件拷贝到项目的../app/src/main/jniLibs/armeabi下
		将BaiduLBS_Android.jar拷贝到../app/libs目录下，并导入项目（Studio默认自动导入)

	6，下载示例代码：http://lbsyun.baidu.com/sdk/download
		找到：地图+检索+LBS云检索+工具+周边雷达（目录）
		找到：/BaiduMapsApiDemo/src/baidumapsdk/demo（目录）
		找到：LocationDemo.java 文件就是定位示例）

	7, 将 LocationDemo.java 导入到项目
		找到：示例项目中的 AndroidManifest.xml （文件）
		取出：下面示例中的内容，拷贝到当前项目中 ...

	8，下面的：LocationActivity.java 是我稍加修改的 LocationDemo.java

	9，先做到可以正常运行官方示例（LocationDemo.java）

	10，需要提醒的是，生成APK需要通过, Build/Generate Signed APK(Studio) 生成
			否则程序的指纹信息无法与安全码进行匹配...

	// 百度地图仅LBS搜索还有着很多功能... 具体请看官方文档
	// http://developer.baidu.com/map/index.php?title=android-locsdk

>

---

>

	在application标签中声明service组件,每个app拥有自己单独的定位service
	<service 
		android:name="com.baidu.location.f" 
		android:enabled="true" 
		android:process=":remote">
	</service>

>

	【重要提醒】
	定位SDKv3.1版本之后，以下权限已不需要，请取消声明
	否则将由于Android 5.0多帐户系统加强权限管理而导致应用安装失败。
	<uses-permission android:name="android.permission.BAIDU_LOCATION_SERVICE"></uses-permission>

>

	// 声明使用权限
	<!-- 这个权限用于进行网络定位-->
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"></uses-permission>
	<!-- 这个权限用于访问GPS定位-->
	<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"></uses-permission>
	<!-- 用于访问wifi网络信息，wifi信息会用于进行网络定位-->
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>
	<!-- 获取运营商信息，用于支持提供运营商信息相关的接口-->
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>
	<!-- 这个权限用于获取wifi的获取权限，wifi信息会用来进行网络定位-->
	<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"></uses-permission>
	<!-- 用于读取手机当前的状态-->
	<uses-permission android:name="android.permission.READ_PHONE_STATE"></uses-permission>
	<!-- 写入扩展存储，向扩展卡写入数据，用于写入离线定位数据-->
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
	<!-- 访问网络，网络定位需要上网-->
	<uses-permission android:name="android.permission.INTERNET" />
	<!-- SD卡读取权限，用户写入离线定位数据-->
	<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"></uses-permission>

>

	SDK4.2及之后版本需要在Mainfest.xml设置Accesskey
	设置有误会引起定位和地理围栏服务不能正常使用，必须进行Accesskey的正确设置。
	设置AccessKey，在application标签中加入
	// key:开发者申请的key
	<meta-data android:name="com.baidu.lbsapi.API_KEY" android:value="key" />

>

---

>

	// 具体获取坐标代码

	// 定位相关
	LocationClient mLocClient;
	public MyLocationListenner myListener = new MyLocationListenner();

	@Override
	public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_location);
		// 定位初始化
		mLocClient = new LocationClient(this);
		mLocClient.registerLocationListener(myListener);
		LocationClientOption option = new LocationClientOption();
		option.setOpenGps(true);// 打开gps
		option.setCoorType("bd09ll"); // 设置坐标类型
		option.setScanSpan(1000);
		mLocClient.setLocOption(option);
		mLocClient.start();
	}

	/**
	 * 定位SDK监听函数
	 */
	public class MyLocationListenner implements BDLocationListener {
		@Override
		public void onReceiveLocation(BDLocation location) {
			MyLocationData locData = new MyLocationData.Builder()
					.accuracy(location.getRadius())
					// 此处设置开发者获取到的方向信息，顺时针0-360
					// 这里的方向需要用户通过传感器自定获取并设置
					.direction(100).latitude(location.getLatitude())
					.longitude(location.getLongitude()).build();
			Log.i("纬度", String.valueOf(locData.latitude));
			Log.i("经度", String.valueOf(locData.longitude));
			Log.i("速度", String.valueOf(locData.speed));
			Log.i("方向", String.valueOf(locData.direction));
			Log.i("精度", String.valueOf(locData.accuracy));
		}
	}

>

---

>

	// 完整的官方SDK定位示例
	public class LocationActivity extends Activity {

		// 定位相关
		LocationClient mLocClient;
		public MyLocationListenner myListener = new MyLocationListenner();
		private LocationMode mCurrentMode;
		BitmapDescriptor mCurrentMarker;

		MapView mMapView;
		BaiduMap mBaiduMap;

		// UI相关
		OnCheckedChangeListener radioButtonListener;
		Button requestLocButton;
		boolean isFirstLoc = true;// 是否首次定位

		@Override
		public void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_location);
			requestLocButton = (Button) findViewById(R.id.button1);
			mCurrentMode = LocationMode.NORMAL;
			requestLocButton.setText("普通");
			OnClickListener btnClickListener = new OnClickListener() {
				public void onClick(View v) {
					switch (mCurrentMode) {
						case NORMAL:
							requestLocButton.setText("跟随");
							mCurrentMode = LocationMode.FOLLOWING;
							mBaiduMap.setMyLocationConfigeration(new MyLocationConfiguration(mCurrentMode, true, mCurrentMarker));
							break;
						case COMPASS:
							requestLocButton.setText("普通");
							mCurrentMode = LocationMode.NORMAL;
							mBaiduMap.setMyLocationConfigeration(new MyLocationConfiguration(mCurrentMode, true, mCurrentMarker));
							break;
						case FOLLOWING:
							requestLocButton.setText("罗盘");
							mCurrentMode = LocationMode.COMPASS;
							mBaiduMap.setMyLocationConfigeration(new MyLocationConfiguration(mCurrentMode, true, mCurrentMarker));
							break;
					}
				}
			};
			requestLocButton.setOnClickListener(btnClickListener);

			RadioGroup group = (RadioGroup) this.findViewById(R.id.radioGroup);
			radioButtonListener = new OnCheckedChangeListener() {
				@Override
				public void onCheckedChanged(RadioGroup group, int checkedId) {
					if (checkedId == R.id.defaulticon) {
						// 传入null则，恢复默认图标
						mCurrentMarker = null;
						mBaiduMap.setMyLocationConfigeration(new MyLocationConfiguration(mCurrentMode, true, null));
					}
					if (checkedId == R.id.customicon) {
						// 修改为自定义marker
						mCurrentMarker = BitmapDescriptorFactory.fromResource(R.drawable.icon_geo);
						mBaiduMap.setMyLocationConfigeration(new MyLocationConfiguration(mCurrentMode, true, mCurrentMarker));
					}
				}
			};
			group.setOnCheckedChangeListener(radioButtonListener);

			// 地图初始化
			mMapView = (MapView) findViewById(R.id.bmapView);
			mBaiduMap = mMapView.getMap();
			// 开启定位图层
			mBaiduMap.setMyLocationEnabled(true);
			// 定位初始化
			mLocClient = new LocationClient(this);
			mLocClient.registerLocationListener(myListener);
			LocationClientOption option = new LocationClientOption();
			option.setOpenGps(true);// 打开gps
			option.setCoorType("bd09ll"); // 设置坐标类型
			option.setScanSpan(1000);
			mLocClient.setLocOption(option);
			mLocClient.start();
		}

		/**
		 * 定位SDK监听函数
		 */
		public class MyLocationListenner implements BDLocationListener {

			@Override
			public void onReceiveLocation(BDLocation location) {
				// map view 销毁后不在处理新接收的位置
				if (location == null || mMapView == null)
					return;
				MyLocationData locData = new MyLocationData.Builder()
						.accuracy(location.getRadius())
								// 此处设置开发者获取到的方向信息，顺时针0-360
								// 这里的方向需要用户通过传感器自定获取并设置
						.direction(100).latitude(location.getLatitude())
						.longitude(location.getLongitude()).build();
				Log.i("纬度", String.valueOf(locData.latitude));
				Log.i("经度", String.valueOf(locData.longitude));
				Log.i("速度", String.valueOf(locData.speed));
				Log.i("方向", String.valueOf(locData.direction));
				Log.i("精度", String.valueOf(locData.accuracy));
				mBaiduMap.setMyLocationData(locData);
				if (isFirstLoc) {
					isFirstLoc = false;
					LatLng ll = new LatLng(location.getLatitude(),
							location.getLongitude());
					MapStatusUpdate u = MapStatusUpdateFactory.newLatLng(ll);
					mBaiduMap.animateMapStatus(u);
				}
			}

			public void onReceivePoi(BDLocation poiLocation) {
			}
		}

		@Override
		protected void onPause() {
			mMapView.onPause();
			super.onPause();
		}

		@Override
		protected void onResume() {
			mMapView.onResume();
			super.onResume();
		}

		@Override
		protected void onDestroy() {
			// 退出时销毁定位
			mLocClient.stop();
			// 关闭定位图层
			mBaiduMap.setMyLocationEnabled(false);
			mMapView.onDestroy();
			mMapView = null;
			super.onDestroy();
		}

	}
