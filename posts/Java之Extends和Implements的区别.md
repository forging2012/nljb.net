---
title: Java之Extends和Implements的区别
date: '2015-04-22'
description:
categories:

tags:java

---

>

### Extends 继承

>

只要类被定义为(final)就是不能被继承的

>

Extends继承一个已有的类,被继承的类称为父类(超类,基类),新的类称为子类(派生类)

>

	1. public继承
		明显父类public成员在子类中仍然是public，所以子类对象可以调用父类的接口
	 
	2. protected继承
		protected继承后，父类public和protected成员都变成子类的protected成员了
		也就是说子类对象无法调用父类的接口，只能将父类的函数当作子类的内部实现
	 
	3. private继承
		private继承后，父类public和protected成员都变成子类的private了，它比protected继承更严格。
		也就说这些父类的成员只能被继承一次，再继续继承，父类的成员就不可见了。

>

	class A {
		int i;
		void f() {}
		void A() {} // 构造
	}

	// B 继承 A
	class B extends A {
		int j;	
		void f() {} // 重写
		void g() {}
		void B() {  // 构造
			// 调用父类的构造方法
			super();
		}
	}

	B b = new B();
	b.i   // 继承的
	b.f() // 重写后的
	b.j   // 自有的
	b.g() // 自有的

>

---

>

### Implements 接口实现

>

Implements是一个类实现一个接口用的关键字，它是用来实现接口中定义的抽象方法

>

interface接口内部全部是由全局常量和公共抽象方法所组成

>

对于class而言，extends用于(单)继承一个类(class)，而implements用于实现一个接口(interface)

>

Implements，实现父类，子类不可以覆盖父类的方法或者变量。

即使子类定义与父类相同的变量或者函数，也会被父类取代掉

>

interface定义一些方法,并没有实现,需要implements来实现才可用

extend可以继承一个接口,但仍是一个接口,也需要implements之后才可用

>

这样的好处是：架构师定义好接口，让工程师实现就可以了。整个项目开发效率和开发成本大大降低。 

>

	// 接口
	public interface People {
		public void say();
	}

	public interface People2 {
		public void say2();
	}

	// 等着被实现
	public class Chinese implements People, People2 {
		public void say() {
			System.out.println(" 你好！");
		}
		public void say2() {
			System.out.println(" 你好！");
		}
	}

	People chinese = new Chinese() ;
	chinese.say();
	chinese.say();

