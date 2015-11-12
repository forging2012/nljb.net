---
title: Android之百度地图获取坐标及地址
date: '2015-11-12'
description:
categories:

tags:android

---

>

### 百度地图获取坐标及地址

>

	// 坐标定位
	public void initLocation() {
		// 定位初始化
		mLocClient = new LocationClient(this);
		mLocClient.registerLocationListener(myListener);
		LocationClientOption option = new LocationClientOption();
		option.setOpenGps(true);// 打开gps
		option.setCoorType("bd09ll"); // 设置坐标类型
		option.setScanSpan(LocationClientOption.MIN_SCAN_SPAN);
		option.setIsNeedAddress(true);
		mLocClient.setLocOption(option);
		mLocClient.start();
	}

>

    /**
     * 定位SDK监听函数
     */
    public class MyLocationListenner implements BDLocationListener {

        @Override
        public void onReceiveLocation(BDLocation location) {
            if (location == null)
                return;
            MyLocationData locData = new MyLocationData.Builder()
                    .accuracy(location.getRadius())
                    .direction(100).latitude(location.getLatitude())
                    .longitude(location.getLongitude()).build();
            Log.i("纬度", String.valueOf(locData.latitude));
            Log.i("经度", String.valueOf(locData.longitude));
            Log.i("速度", String.valueOf(locData.speed));
            Log.i("方向", String.valueOf(locData.direction));
            Log.i("精度", String.valueOf(locData.accuracy));
            Log.i("地址", String.valueOf(location.getAddress().city));
            // ...
            if (location.getAddress() != null && location.getAddress().city != null) {
                // 销毁定位
                mLocClient.stop();
            }
        }

        public void onReceivePoi(BDLocation poiLocation) {
        }
    }

>

