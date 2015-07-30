---
title: Android之百度地图兴趣点搜索
date: '2015-07-30'
description:
categories:

tags:android

---

>

### 百度地图兴趣点搜索

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
		找到：PoiSearchDemo.java （文件就是兴趣点搜索示例）

	7, 将 PoiSearchDemo.java 导入到项目
		找到：示例项目中的 AndroidManifest.xml （文件）
		取出：下面示例中的内容，拷贝到当前项目中 ...
		
	8，下面的：PoiSearchActivity 是我稍加修改的 PoiSearchDemo

	9，这样通过指定：city（城市）和（searchkey）地址，就可以进行搜索了

	10，需要提醒的是，生成APK需要通过, Build/Generate Signed APK(Studio) 生成
			否则程序的指纹信息无法与安全码进行匹配...

>

---

>

    <application
		...
		<activity android:name=".PoiSearchDemo" />
        <meta-data
            android:name="com.baidu.lbsapi.API_KEY"
            android:value="OCEmzFbp7EA8LOFmUzzMkHnX" />
        <service
            android:name="com.baidu.location.f"
            android:enabled="true"
            android:process=":remote" >
        </service>
		...
    </application>

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" >
    </uses-permission>
    <uses-permission android:name="android.permission.INTERNET" >
    </uses-permission>
    <uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />
    <uses-permission android:name="android.permission.WAKE_LOCK" >
    </uses-permission>
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <!-- SDK1.5需要android.permission.GET_TASKS权限判断本程序是否为当前运行的应用? -->
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" >
    </uses-permission>
    <uses-permission android:name="android.permission.WRITE_SETTINGS" />
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
	<!--允许应用读取低级别的系统日志文件 -->
	<uses-permission android:name="android.permission.READ_LOGS"></uses-permission>
 
    <supports-screens
        android:anyDensity="true"
        android:largeScreens="true"
        android:normalScreens="false"
        android:resizeable="true"
        android:smallScreens="true" />

>

---

>

### 官方演示（稍加修改)

>

	/**
	 * poi搜索功能
	 */
	public class PoiSearchActivity extends FragmentActivity implements
			OnGetPoiSearchResultListener, OnGetSuggestionResultListener {

		/**
		 * city         城市
		 * searchkey    地址
		 */
		private PoiSearch mPoiSearch = null;
		private SuggestionSearch mSuggestionSearch = null;
		private BaiduMap mBaiduMap = null;
		/**
		 * 搜索关键字输入窗口
		 */
		private AutoCompleteTextView keyWorldsView = null;
		private ArrayAdapter<String> sugAdapter = null;
		private int load_Index = 0;
		/**
		 * 返回
		 */
		private ImageView backing;

		@Override
		public void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_poisearch);
			// 返回
			backing = (ImageView) findViewById(R.id.car_backing);
			backing.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View v) {
					finish();
				}
			});
			// 初始化搜索模块，注册搜索事件监听
			mPoiSearch = PoiSearch.newInstance();
			mPoiSearch.setOnGetPoiSearchResultListener(this);
			mSuggestionSearch = SuggestionSearch.newInstance();
			mSuggestionSearch.setOnGetSuggestionResultListener(this);
			keyWorldsView = new AutoCompleteTextView(this);
			// keyWorldsView = (AutoCompleteTextView) findViewById(R.id.searchkey);
			sugAdapter = new ArrayAdapter<String>(this,
					android.R.layout.simple_dropdown_item_1line);
			keyWorldsView.setAdapter(sugAdapter);
			mBaiduMap = ((SupportMapFragment) (getSupportFragmentManager()
					.findFragmentById(R.id.map))).getBaiduMap();
			// 获取搜索地址
			Intent intent = getIntent();
			String city = intent.getStringExtra("POI_SEARCH_CITY");
			String key = intent.getStringExtra("POI_SEARCH_KEY");
			// 自动搜索
			searchButtonProcess(city, key);
			/**
			 * 当输入关键字变化时，动态更新建议列表
			 */
	//        keyWorldsView.addTextChangedListener(new TextWatcher() {
	//
	//            @Override
	//            public void afterTextChanged(Editable arg0) {
	//
	//            }
	//
	//            @Override
	//            public void beforeTextChanged(CharSequence arg0, int arg1,
	//                                          int arg2, int arg3) {
	//
	//            }
	//
	//            @Override
	//            public void onTextChanged(CharSequence cs, int arg1, int arg2,
	//                                      int arg3) {
	//                if (cs.length() <= 0) {
	//                    return;
	//                }
	//                String city = ((EditText) findViewById(R.id.city)).getText()
	//                        .toString();
	//                /**
	//                 * 使用建议搜索服务获取建议列表，结果在onSuggestionResult()中更新
	//                 */
	//                mSuggestionSearch
	//                        .requestSuggestion((new SuggestionSearchOption())
	//                                .keyword(cs.toString()).city(city));
	//            }
	//        });

		}

		@Override
		protected void onPause() {
			super.onPause();
		}

		@Override
		protected void onResume() {
			super.onResume();
		}

		@Override
		protected void onDestroy() {
			mPoiSearch.destroy();
			mSuggestionSearch.destroy();
			super.onDestroy();
		}

		@Override
		protected void onSaveInstanceState(Bundle outState) {
			super.onSaveInstanceState(outState);
		}

		@Override
		protected void onRestoreInstanceState(Bundle savedInstanceState) {
			super.onRestoreInstanceState(savedInstanceState);
		}

		/**
		 * 影响搜索按钮点击事件
		 */
		public void searchButtonProcess(String city, String searchkey) {
	//        EditText editCity = (EditText) findViewById(R.id.city);
	//        EditText editSearchKey = (EditText) findViewById(R.id.searchkey);
	//        mPoiSearch.searchInCity((new PoiCitySearchOption())
	//                .city(editCity.getText().toString())
	//                .keyword(editSearchKey.getText().toString())
	//                .pageNum(load_Index));
			mPoiSearch.searchInCity((new PoiCitySearchOption())
					.city(city)
					.keyword(searchkey)
					.pageNum(load_Index));
		}

		public void goToNextPage(View v) {
			load_Index++;
			// searchButtonProcess();
		}

		public void onGetPoiResult(PoiResult result) {
			if (result == null
					|| result.error == SearchResult.ERRORNO.RESULT_NOT_FOUND) {
				Toast.makeText(PoiSearchActivity.this, "未找到结果", Toast.LENGTH_LONG)
						.show();
				return;
			}
			if (result.error == SearchResult.ERRORNO.NO_ERROR) {
				mBaiduMap.clear();
				PoiOverlay overlay = new MyPoiOverlay(mBaiduMap);
				mBaiduMap.setOnMarkerClickListener(overlay);
				overlay.setData(result);
				overlay.addToMap();
				overlay.zoomToSpan();
				return;
			}
			if (result.error == SearchResult.ERRORNO.AMBIGUOUS_KEYWORD) {

				// 当输入关键字在本市没有找到，但在其他城市找到时，返回包含该关键字信息的城市列表
				String strInfo = "在";
				for (CityInfo cityInfo : result.getSuggestCityList()) {
					strInfo += cityInfo.city;
					strInfo += ",";
				}
				strInfo += "找到结果";
				Toast.makeText(PoiSearchActivity.this, strInfo, Toast.LENGTH_LONG)
						.show();
			}
		}

		public void onGetPoiDetailResult(PoiDetailResult result) {
			if (result.error != SearchResult.ERRORNO.NO_ERROR) {
				Toast.makeText(PoiSearchActivity.this, "抱歉，未找到结果", Toast.LENGTH_SHORT)
						.show();
			} else {
				Toast.makeText(PoiSearchActivity.this, result.getName() + ": " + result.getAddress(), Toast.LENGTH_SHORT)
						.show();
			}
		}

		@Override
		public void onGetSuggestionResult(SuggestionResult res) {
			if (res == null || res.getAllSuggestions() == null) {
				return;
			}
			sugAdapter.clear();
			for (SuggestionResult.SuggestionInfo info : res.getAllSuggestions()) {
				if (info.key != null)
					sugAdapter.add(info.key);
			}
			sugAdapter.notifyDataSetChanged();
		}

		private class MyPoiOverlay extends PoiOverlay {

			public MyPoiOverlay(BaiduMap baiduMap) {
				super(baiduMap);
			}

			@Override
			public boolean onPoiClick(int index) {
				super.onPoiClick(index);
				PoiInfo poi = getPoiResult().getAllPoi().get(index);
				// if (poi.hasCaterDetails) {
				mPoiSearch.searchPoiDetail((new PoiDetailSearchOption())
						.poiUid(poi.uid));
				// }
				return true;
			}
		}
	}


