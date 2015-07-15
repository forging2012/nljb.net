---
title: Android之zxing扫描二维码画面拉伸问题
date: '2015-07-15'
description:
categories:

tags:android

---

>

### zxing扫描二维码画面拉伸

>

解决方法：修改 CameraConfigurationManager.java 文件

>

	  /**
	   * Reads, one time, values from the camera that are needed by the app.
	   */
	  void initFromCameraParameters(Camera camera) {
		Camera.Parameters parameters = camera.getParameters();
		previewFormat = parameters.getPreviewFormat();
		previewFormatString = parameters.get("preview-format");
		Log.d(TAG, "Default preview format: " + previewFormat + '/' + previewFormatString);
		WindowManager manager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
		Display display = manager.getDefaultDisplay();
		screenResolution = new Point(display.getWidth(), display.getHeight());
		Log.d(TAG, "Screen resolution: " + screenResolution);
		Point screenResolutionForCamera = new Point();
		screenResolutionForCamera.x = screenResolution.x;
		screenResolutionForCamera.y = screenResolution.y;
		// preview size is always something like 480*320, other 320*480
		if (screenResolution.x < screenResolution.y) {
			screenResolutionForCamera.x = screenResolution.y;
			screenResolutionForCamera.y = screenResolution.x;
		}
		// cameraResolution = getCameraResolution(parameters, screenResolution);
		cameraResolution = getCameraResolution(parameters, screenResolutionForCamera);
		Log.d(TAG, "Camera resolution: " + screenResolution);
	  }

