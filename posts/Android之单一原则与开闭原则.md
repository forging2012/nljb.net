---
title: Android之单一原则与开闭原则
date: '2016-02-02'
description:
categories:

tags:android

---

>

### 单一原则

>

	// 案例
	public class ImageLoader {

		// 图片缓存
		LruCache<String, Bitmap> mImageCache;

		// 构造
		public ImageLoader() {
			// 初始化
			initImageCache()	
		}

		// 初始化
		private void initImageCache() {
			...
			// 计算可使用的最大内存
			int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);
			// 取四分之一的可用内存作为缓存
			int cacheSize = maxMemory / 4;
			// 初始化图片缓存
			mImageCache = new LruCache<String, Bitmap>(cacheSize) {
				...
			}
		}

		// 显示Image
		public void displayImage(String url, ImageView imageView) {
			...
			Bitmap bitmap = mImageCache.get(url);
			if (bitmap != null) {
				imageView.setImageBitmap(bitmap);
			} else {
				// ... new Runnable() ...
				Bitmap bitmap = downloadImage(url);
				if (bitmap == null) {
					return;
				} else {
					imageView.setImageBitmap(bitmap);
					miMageCache.put(url, bitmap);
				}
			}
		}

		// 下载Image
		public Bitmap downloadImage(String imageUrl) {
			...
		}

	}

>

---

>

	// 单一原则所表达出的用意就是单一二字
	// 两个完全不一样的功能就不应该放在一个类中...
	// 一个类中应该是一组相关性很高的函数、数据的封装 ...

>

	public class ImageLoader {

		ImageCache mImageCache = new ImageCache();

		public void displayImage(String url, ImageView imageView) {
			...
		}

		public Bitmap downloadImage(String imageUrl) {
			...
		}

	}

	public class ImageCache {

		LruCache<String, Bitmap> mImageCache;

		private void initImageCache() {
			...
		}

		public void put(String url, Bitmap bitmap) {	
			mImageCache.put(url, bitmap);
		}

		public Bitmap get(String url) {
			return mImageCache.get(url);
		}

	}

>

---

>

### 开闭原则

>

	// 开闭原则（Open Close Principle) OCP 
	// 开闭原则的定义是：软件中的对象（类，模块，函数）
	// 应该对于扩展是开放的，对于修改是封闭的 ...

	public class ImageLoader {

		// 默认图片缓存
		ImageCache mImageCache = new MemoryCache();
		
		// 注入缓存实现
		public void setImageCache(ImageCache cache) {
			mImageCache = cache;
		}

		public void displayImage(String imageUrl, ImageView imageView) {
			Bitmap bitmap = mImageCache.get(imageUrl);
			if (bitmap != null) {
				imageView.setImageBitmap(bitmap);
				return;
			}
			Bitmap bitmap = downloadImage(imageUrl);
			if (bitmap == null) {
				return;
			} else {
				imageView.setImageBitmap(bitmap);
				mImageCache.put(imageUrl, bitmap);
		}

		public Bitmap downloadImage(String imageUrl) {
		}	

	}

	// ImageCache 接口
	public interface ImageCache {
		public Bitmap get(String url);
		public void put(String url, Bitmap bmp);
	}

	// 实现类 +1
	public class MemoryCache implements ImageCache {

		private LruCache<String, Bitmap> mMemeryCache;

		@Override
		public Bitmap get(String url) {
			return mMemeryCache.get(url);
		}

		@Override
		public void put(String url, Bitmap bmp) {
			mMemeryCache.put(url, bmp);	
		}

	}

	// 实现类 +1
	public class DiskCache implements ImageCache {
		...
	}

	// 实现类 +1
	public class DoubleCache implements ImageCache {
		...
	}

>

	ImageLoader imageLoader = new ImageLoader();

	// 使用内存缓存
	imageLoader.setImageCache(new MemoryCache());

	// 使用SD卡缓存
	imageLoader.setImageCache(new DiskCache());

	// 使用双缓存
	imageLoader.setImageCache(new DoubleCache());

	// 使用自定义缓存
	imageLoader.setImageCache(new ImageCache() {
		
		@Override
		public void put(String url, Bitmap bmp) {
			...
		}

		@Override
		public Bitmap get(String url) {
			...
		}

	});

>

