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


>

---

>

### 修改扫描窗口大小

>

	在CameraManager.java这个类中进行调整

	默认的大小是 以下这4个参数 
	//  private static final int MIN_FRAME_WIDTH = 240;
	//  private static final int MIN_FRAME_HEIGHT = 240;
	//  private static final int MAX_FRAME_WIDTH = 480;
	//  private static final int MAX_FRAME_HEIGHT = 360;

	参数实际在 getFramingRect() 方法中起作用
    原始：

    /** 
      * Calculates the framing rect which the UI should draw to show the user where to place the 
      * barcode. This target helps with alignment as well as forces the user to hold the device 
      * far enough away to ensure the image will be in focus. 
      * 
      * @return The rectangle to draw on screen in window coordinates. 
      */  
     public Rect getFramingRect() {  
       Point screenResolution = configManager.getScreenResolution();  
       if (framingRect == null) {  
         if (camera == null) {  
           return null;  
         }  

         //原生  
         int width = screenResolution.x * 3 / 4;  
         if (width < MIN_FRAME_WIDTH) {  
           width = MIN_FRAME_WIDTH;  
         } else if (width > MAX_FRAME_WIDTH) {  
           width = MAX_FRAME_WIDTH;  
         }  
         int height = screenResolution.y * 3 / 4;  
         if (height < MIN_FRAME_HEIGHT) {  
           height = MIN_FRAME_HEIGHT;  
         } else if (height > MAX_FRAME_HEIGHT) {  
           height = MAX_FRAME_HEIGHT;  
         }  
         int leftOffset = (screenResolution.x - width) / 2;  
         int topOffset = (screenResolution.y - height) / 2;  
         framingRect = new Rect(leftOffset, topOffset, leftOffset + width, topOffset + height);  
         Log.d(TAG, "Calculated framing rect: " + framingRect);  
       }  
       return framingRect;  
     }  
     
>

	改为:
    
    public Rect getFramingRect() {  
      Point screenResolution = configManager.getScreenResolution();  
      if (framingRect == null) {  
        if (camera == null) {  
          return null;  
        }  

      //修改之后    
      int width = screenResolution.x * 7 / 10;  
      int height = screenResolution.y * 7 / 10;  

      int leftOffset = (screenResolution.x - width) / 2;  
      int topOffset = (screenResolution.y - height) / 3;  
      framingRect = new Rect(leftOffset, topOffset, leftOffset + width, topOffset + height);  


        Log.d(TAG, "Calculated framing rect: " + framingRect);  
      }  
      return framingRect;  
    }  
