---
title: Android之单例模式
date: '2016-02-03'
description:
categories:

tags:android

---

>

### 单例模式

>

	单例模式是设计模式中比较简单，只有一个单例类
	没有其它的层次结构与抽象，该模式需要确保该类
	只能生成一个对象，通常是需要消耗较多的资源...

>
	
	例如: 一个应用只有一个Application对象

>

	// 单例模式
	public class CEO {

		private static final CEO mCeo = new CEO();

		public static CEO getCeo() {
			return mCeo;
		}

	}

>

	// 懒汉模式
	public class Singleton {
		
		private static Singleton instance;

		private Singleton() {
		}

		public static synchronized Singleton getInstance() {
			// 第一次调用的时候会被初始化
			if (instance == null) {
				instance = new Singleton();
			}
			return instance;
		}

	}

>

	// Double Check Lock (DCL) 单例模式
	public class Singleton {
		
		private static Singleton mInstance = null;

		private Singleton() {
		}

		public void doSomething() {
			System.out.println("do sth.");
		}

		public static Singleton getInstance() {
			// 避免不必要的同步
			if (mInstance == null) {
				// 同步
				synchronized (Singleton.class) {
					// 在第一次调用时初始化
					if (mInstance == null) {
						mInstance = new Singleton();
					}
					return mInstance;
				}
			}
		}
	}

>

	// 静态内部类单例模式
	public class Singleton {

		private Singleton() {
		}

		public static Singleton getInstance() {
			return SingletonHolder.sInstance;
		}

		// 静态内部类
		private static class SingletonHolder {
			private static final Singleton sInstance = new Singleton();
		}

	}

>

---

>

	// 运用单例模式
	public void initImageLoader(Context context) {
		ImageLoaderConfiguration config = new ImageLoaderConfiguration.Builder( ..... );
		ImageLoader.getInstance().init(config);
		ImageLoader.getInstance().displayImage("图片.url", mImageView);
	}

	public final class ImageLoader {
		
		private static ImageLoader sInstance;

		private ImageLoaderConfig = mConfig;

		... // 省略若干
		
		public static ImageLoader getInstance() {
			if (sInstance == null) {
				synchronized (ImageLoader.class) {
					if (sInstance == null) {
						sInstance = new ImageLoader();
					}
				}
			}
			return sInstance;
		}

		public void init(ImageLoaderConfig config) {
			mConfig = config;
			... // 省略若干
		}

		public void displayImage(String url, ImageView imageView) {
			// 通过获取 mConfig.XXX 来处理获取到的 Image 
			// 随后将图片设置到ImageView ...
			// ... 省略若干
		}

	}

>

