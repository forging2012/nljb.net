---
title: Android之里氏替换原则
date: '2016-02-02'
description:
categories:

tags:android

---

>

### 里氏替换原则

>

	// 里氏替换原则全称是 Liskov Substitution Principle
	// 缩写是，LSP 

	// 面向对象的豫园的三大特点是：继承、封装、多态
	// 里氏替换原则就是依赖于继承、多态这两大特性

	// 所有引用基类的地方必须能透明地使用其子类的对象
	// 只要父类能出现的地方子类就可以出现
	// 而且替换为子类也不会产生任何错误或异常
	// 使用者可能根本就不需要知道是父类还是子类
	// 反过来就不行了，有子类出现的地方，父类未必可行

	public class Window {

		// 传入View类型
		public void show(View view) {	
			// 所有View类型都拥有draw方法
			view.draw();
		}

	}

	// 抽象类
	public abstract class View {

		// 所有继承 Viwe 的类都必须实现 draw
		public abstract void draw();

		// 所有子类共享该方法 		
		public void measure(int width, int height) {
			// ...
		}

	}

	// 继承 View
	public class Button extends View {

		// 继承 abstract 必须实现 draw
		public void draw() {
			// ... 不同的绘制
		}

	}

	// 继承 View
	public class TextView extends View {

		// 继承 abstract 必须实现 draw
		public void draw() {
			// ... 不同的绘制
		}

	}

	// Window 依赖于View, 而View定义了一个视图抽象
	// measure 是各个子类共享的方法
	// 子类通过覆写Viwe的draw方法实现具有各自特色的功能 
	// 任何继承自View类的子类都可以设置给show方法
	// 就是所说的里氏替换原则 ...

	// 继承的优点:
		(1) 代码重用，减少创建类的成本，每个子类都拥有父类方法属性
		(2) 子类与父类基本相似，但又与父类有所区别
		(3) 提高代码的可扩展性
	// 继承的缺点:
		(1) 继承是侵入的，只要继承就必须拥有父亲的所有属性和方法
		(2) 可能造成子类冗余，灵活度降低，因为子类必须拥有父类的属性和方法

	// 接着上面的 ImageLoader 来说明：
		既 MemoryCache、DiskCache、DoubleCache 
		都可以替换ImageCache的工作，并且能够保证行为的正确性

>

