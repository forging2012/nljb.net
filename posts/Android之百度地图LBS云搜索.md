---
title: Android之百度地图LBS云搜索
date: '2015-07-31'
description:
categories:

tags:android

---

>

### 百度地图LBS云搜索

>

	1，在百度开放平台创建应用，并且设置安全码
		地址：http://lbsyun.baidu.com/apiconsole/key

	2，获取应用访问应用（AK）...
		这里与兴趣点不同，同时需要服务器(AK)

	3，从百度开放平台下载SDK
		地址：http://lbsyun.baidu.com/sdk/download

	4，基础SDK开发包：定位功能，基础地图，检索功能，LBS云检索
		获取文件：BaiduLBS_Android.jar + armeabi（目录）

	5，将armeabi目录内文件拷贝到项目的../app/src/main/jniLibs/armeabi下
		将BaiduLBS_Android.jar拷贝到../app/libs目录下，并导入项目（Studio默认自动导入)

	6，下载示例代码：http://lbsyun.baidu.com/sdk/download
		找到：地图+检索+LBS云检索+工具+周边雷达（目录）
		找到：/BaiduMapsApiDemo/src/baidumapsdk/demo（目录）
		找到：CloudSearchActivity.java 文件就是LBS云搜索示例）

	7, 将 CloudSearchActivity.java 导入到项目
		找到：示例项目中的 AndroidManifest.xml （文件）
		取出：下面示例中的内容，拷贝到当前项目中 ...

	8，下面的：CloudSearchActivity.java 是我稍加修改的 CloudSearchActivity.java

	9，先做到可以正常运行官方示例（CloudSearchActivity.java）

	10，需要提醒的是，生成APK需要通过, Build/Generate Signed APK(Studio) 生成
			否则程序的指纹信息无法与安全码进行匹配...

	// 百度地图仅LBS搜索还有着很多功能... 具体请看官方文档
	// http://developer.baidu.com/map/index.php?title=lbscloud/api/geosearch

>

---

>

### 云搜索具体介绍

>

	云搜索分为：云存储，云搜索
	云存储：将需要被搜索到的POI点存储到百度云数据中
	云搜索：通过设置的POI点ID及相关参数进行搜索，比如：坐标，标签等
	// 云存储地址：http://lbsyun.baidu.com/datamanager/datamanage

>

	云搜索分为：本地搜索，周边搜索，矩形搜索，详情检索
	本地搜索：通过，城市+关键字+标签，在存储中搜索匹配数据
	周边搜索：通过，当前坐标+范围+标签，在存储中搜索匹配数据
	矩形搜索：通过，指定区域坐标+关键字+标签，在存储中搜索匹配数据
	// 官方文档：http://developer.baidu.com/map/index.php?title=lbscloud/api/geosearch

>

	// 在百度云存储数据进行设置

<img src="{{urls.media}}/Android之百度地图LBS云搜索/1.png" alt="" width="700" hight="350" >

>

	// 在手机地图中显示的效果

<img src="{{urls.media}}/Android之百度地图LBS云搜索/2.jpg" alt="" width="350" hight="600" >

>

---

>

### LBS云搜索示例

>

	public class CloudSearchActivity extends Activity implements CloudListener {
		private static final String LTAG = CloudSearchActivity.class
				.getSimpleName();
		private MapView mMapView;
		private BaiduMap mBaiduMap;

		@Override
		protected void onCreate(Bundle icicle) {
			super.onCreate(icicle);
			setContentView(R.layout.activity_lbssearch);
			CloudManager.getInstance().init(CloudSearchActivity.this);
			mMapView = (MapView) findViewById(R.id.bmapView);
			mBaiduMap = mMapView.getMap();
			findViewById(R.id.regionSearch).setOnClickListener(
					new OnClickListener() {
						@Override
						public void onClick(View v) {
							/**
							 * 本地检索是指可检索指定区域范围内的poi信息
							 * 区域通过region参数来设定
							 * 可以是全国范围也可以是小范围的如海淀区。
							 * 检索时可通过tags参数指定检索类型；
							 * 通过sortby参数对检索结果进行排序（支持多字段排序）；
							 * filter参数可以完成对指定数据范围的筛选。
							 */
							LocalSearchInfo info = new LocalSearchInfo();
							// AK 需要使用服务器AK
							info.ak = "eU8VKgzQQSwVCAq1AXDRkHmq";
							// 指定LBS云可以搜索的坐标组ID
							info.geoTableId = 115616;
							// 在LBS云可以设置TAGS类型
							info.tags = "公司";
							// 在指定的地区搜索geoTableId内符合的坐标
							info.q = "海淀区";
							// 在指定的城市搜索geoTableId内符合的坐标
							info.region = "北京市";
							// ...
							CloudManager.getInstance().localSearch(info);
						}
					});
			findViewById(R.id.nearbySearch).setOnClickListener(
					new OnClickListener() {
						public void onClick(View v) {
							/**
							 * 周边检索是指以一点为中心（中心点通过location参数指定）
							 * 搜索中心点附近指定距离范围（搜索半径通过radius参数指定）内的POI点。
							 * 检索时可通过tags参数指定检索的类型；
							 * 通过sortby参数进行检索结果的排序（支持多字段排序）；
							 * filter参数可以完成对指定数据范围的筛选。
							 */
							NearbySearchInfo info = new NearbySearchInfo();
							// AK 需要使用服务器AK
							info.ak = "eU8VKgzQQSwVCAq1AXDRkHmq";
							// 指定LBS云可以搜索的坐标组ID
							info.geoTableId = 115616;
							// 搜索的范围
							info.radius = 1000;
							// 搜索的类型
							info.tags = "企业";
							// 搜索的起始坐标
							info.location = "116.315047,40.043694";
							// ...
							CloudManager.getInstance().nearbySearch(info);
						}
					});

			findViewById(R.id.boundsSearch).setOnClickListener(
					new OnClickListener() {
						public void onClick(View v) {
							/**
							 * 矩形检索是指可检索指定矩形范围内的poi信息
							 * 检索区域通过bounds参数设定的矩形的左下角和右上角的经纬度坐标来确定。
							 * 检索时可通过tags参数指定检索类型；
							 * 检索结果可通过sortby参数进行排序（支持多字段排序）；
							 * 可通过filter参数筛选出指定的数据范围的结果。
							 */
							BoundSearchInfo info = new BoundSearchInfo();
							info.ak = "B266f735e43ab207ec152deff44fec8b";
							info.geoTableId = 31869;
							info.q = "天安门";
							info.bound = "116.401663,39.913961;116.406529,39.917396";
							CloudManager.getInstance().boundSearch(info);
						}
					});
			findViewById(R.id.detailsSearch).setOnClickListener(
					new OnClickListener() {
						public void onClick(View v) {
							/**
							 * 坐标点的详情信息
							 */
							DetailSearchInfo info = new DetailSearchInfo();
							info.ak = "eU8VKgzQQSwVCAq1AXDRkHmq";
							info.geoTableId = 115616;
							info.uid = 1128794574;
							CloudManager.getInstance().detailSearch(info);
						}
					});
		}

		@Override
		protected void onDestroy() {
			super.onDestroy();
			mMapView.onDestroy();
			CloudManager.getInstance().destroy();
		}

		@Override
		protected void onPause() {
			super.onPause();
			mMapView.onPause();
		}

		@Override
		protected void onResume() {
			super.onResume();
			mMapView.onResume();
		}

		public void onGetDetailSearchResult(DetailSearchResult result, int error) {
			if (result != null) {
				if (result.poiInfo != null) {
					Toast.makeText(CloudSearchActivity.this, result.poiInfo.title,
							Toast.LENGTH_SHORT).show();
				} else {
					Toast.makeText(CloudSearchActivity.this,
							"status:" + result.status, Toast.LENGTH_SHORT).show();
				}
			}
		}

		public void onGetSearchResult(CloudSearchResult result, int error) {
			if (result != null && result.poiList != null
					&& result.poiList.size() > 0) {
				Log.d(LTAG, "onGetSearchResult, result length: " + result.poiList.size());
				mBaiduMap.clear();
				BitmapDescriptor bd = BitmapDescriptorFactory.fromResource(R.drawable.icon_gcoding);
				LatLng ll;
				Builder builder = new Builder();
				for (CloudPoiInfo info : result.poiList) {
					ll = new LatLng(info.latitude, info.longitude);
					OverlayOptions oo = new MarkerOptions().icon(bd).position(ll);
					mBaiduMap.addOverlay(oo);
					builder.include(ll);
				}
				LatLngBounds bounds = builder.build();
				MapStatusUpdate u = MapStatusUpdateFactory.newLatLngBounds(bounds);
				mBaiduMap.animateMapStatus(u);
			}
		}
	}


