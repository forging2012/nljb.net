---
title: Android之委托模式开发
date: '2015-07-04'
description:
categories:

tags:android

---

>

### 委托模式

>

也就是将本类中定义调用的方法委托给另外一个类实现

>

	// 委托
	public class Abc {

		private AbcDelegate mAbcDelegate;
	
		// 委托方法
		public static interface AbcDelegate {
			String getData();
		}

		// 初始化
		public Abc(AbcDelegate mAbcDelegate) {
			this.mAbcDelegate = mAbcDelegate;
		}

		public void start() {
			String str = mAbcDelegate.getData();
			Log.i("INFO", str);
		}

	}
	
	// 使用
	public class Cde implements Abc.AbcDelegate {

		private Abc abc = new Abc(this);

		public Cde() {
			abc.start();
		}

		@Override
		public String getData() {
			return "Hello";
		}

	}

