---
title: Android之abstract和interface介绍
date: '2015-07-05'
description:
categories:

tags:android

---

>

### abstract , interface

>

	abstract class Abc {

		public abstract void getData();

		public void Print() {
			getData();
			System.out.print("+\n");
		}

	}

	class Bcd extends Abc {

		@Override
		public void getData() {
			System.out.print("-\n");
		}
		
	}

	class Main {

		/*
			输出
			-
			+
		 */
		public Main() {
			Bcd bcd = new Bcd();
			bcd.Print();
		}

	}

>

abstract class和interface在Java语言中都是用来进行抽象类定义.

>

---

>

使用abstract class的方式定义Demo抽象类的方式如下：

>
	abstract class Demo ｛
		abstract void method1();
		abstract void method2();
		…
	｝

>

使用interface的方式定义Demo抽象类的方式如下：

>
	interface Demo {
		void method1();
		void method2();
		…
	}

>

在abstract class方式中，Demo可以有自己的数据成员，也可以有非abstarct的成员方法

而在interface方式的实现中，Demo只能够有静态的不能被修改的数据成员（也就是必须是static final的）

所有的成员方法都是abstract的。从某种意义上说，interface是一种特殊形式的abstract class。

>

---

>

考虑这样一个例子，假设在我们的问题领域中有一个关于Door的抽象概念，该Door具有执行两个动作open和close

此时我们可以通过abstract class或者interface来定义一个表示该抽象概念的类型，定义方式分别如下所示：

>

使用abstract class方式定义Door：

>
	abstract class Door {
			abstract void open();
			abstract void close()；
	}

>

使用interface方式定义Door：

>

	interface Door {
		void open();
		void close();
	}

>

其他具体的Door类型可以extends使用abstract class方式定义的Door或者implements使用interface方式定义的Door。

看起来好像使用abstract class和interface没有大的区别。

>

---

>

如果现在要求Door还要具有报警的功能。我们该如何设计针对该例子的类结构呢

>

解决方案一：

>

简单的在Door的定义中增加一个alarm方法，如下：

>

	abstract class Door {
			abstract void open();
			abstract void close()；
			abstract void alarm();
	}

>
	或者

>

	interface Door {
			void open();
		void close();
		void alarm();
	}

>

那么具有报警功能的AlarmDoor的定义方式如下：

>

	class AlarmDoor extends Door {
			void open() { … }
			void close() { … }
			void alarm() { … }
	}

>

	或者

>

	class AlarmDoor implements Door ｛
		void open() { … }
		void close() { … }
		void alarm() { … }
	｝

>

这种方法违反了面向对象设计中的一个核心原则ISP（Interface Segregation Priciple）

在Door的定义中把Door概念本身固有的行为方法和另外一个概念"报警器"的行为方法混在了一起。

这样引起的一个问题是那些仅仅依赖于Door这个概念的模块会因为"报警器"这个概念的改变而改变，反之依然。

>

解决方案二：

>

既然open、close和alarm属于两个不同的概念，根据ISP原则应该把它们分别定义在代表这两个概念的抽象类中。

定义方式有：这两个概念都使用abstract class方式定义；

两个概念都使用interface方式定义；一个概念使用abstract class方式定义，另一个概念使用interface方式定义。

>

方式定义。如下所示：

>

	abstract class Door {
			abstract void open();
			abstract void close()；
	}

	interface Alarm {
		void alarm();
	}

	class AlarmDoor extends Door implements Alarm {
		void open() { … }
		void close() { … }
		void alarm() { … }
	}

>

这种实现方式基本上能够明确的反映出我们对于问题领域的理解，正确的揭示我们的设计意图。

其实abstract class表示的是"is a"关系, interface表示的是"like a"关系，大家在选择时可以作为一个依据 

>

abstract class和interface是Java语言中的两种定义抽象类的方式，它们之间有很大的相似性。

但是对于它们的选择却又往往反映出对于问题领域中的概念本质的理解

对于设计意图的反映是否正确、合理，因为它们表现了概念间的不同的关系（虽然都能够实现需求的功能）。

这其实也是语言的一种的惯用法，希望读者朋友能够细细体会。

