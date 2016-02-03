---
title: Android之依赖倒置原则
date: '2016-02-03'
description:
categories:

tags:android

---

>

### 依赖倒置原则

>

	（1）高层模块不应该依赖低层模块，两者都应该依赖其抽象
	（2）抽象不应该依赖细节
	（3）细节应该依赖抽象

>

	抽象：就是指接口或者抽象类，两者都不能直接被实例化的；
	细节：就是实现类，实现接口或者继承抽象类而产生的类
		  特点就是: 
					可以直接被实例化
					可以加上关键字 new 产生对象

>

	模块间的依赖通过抽象发生
	实现类之间不发生直接的依赖关系
	其依赖关系是通过借口或抽象类产生的

>

---

>

	// 例子：依赖于细节
	public class ImageLoader {

		// （直接依赖于细节）
		DoubleCache mCache = new DoubleCache();

		public void displayImage(String url, ImageView imageView) {
			...
		}

		// (直接依赖于细节）
		public void setImageCache(DoubleCache cache) {
			mCache = cache;
		}

	}

	// 抽象：接口
	public interface ImageCache {
		...	
	}


	// 例子：依赖于抽象
	public class ImageLoader {

		// 依赖于抽象（接口或者抽象类）
		ImageCache mCache = new MemoryCache();

		// 设置ImageCache依赖于抽象
		public void setImageCache(ImageCache cache) {
			mCache = cache;
		}

		public void displayImage(String url, ImageView imageView) {
			...
		}

	}

>
