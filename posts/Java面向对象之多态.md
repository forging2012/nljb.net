---
title: Java面向对象之多态
date: '2015-04-23'
description:
categories:

tags:java

---

>

### Java面向对象之多态

>

	向上转型：程序会自动完成
		父类 父类对象 = 子类实例

	向下转型：强制类型转换
		子类 子类对象 = (子类)父类实例

>

	instanceof关键字可以判断一个对象是不是一个类的实例

>

	class A {

	    public void printA() {
		System.out.println("A - print");
	    }
	}


	class B extends A {

	    public void printB() {
		System.out.println("B - print");
	    }

	}

	class C extends A {

	    public void printC() {
		System.out.println("C - print");
	    }

	}

	public class Demo {

	    public void main() {
		A a  = new A();
		System.out.println(a instanceof A);
		A a1 = new B();
		System.out.println(a1 instanceof A);
		System.out.println(a1 instanceof B);

	    }

	}


