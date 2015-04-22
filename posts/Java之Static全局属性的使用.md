---
title: Java之Static全局属性的使用
date: '2015-04-22'
description:
categories:

tags:java

---

>

### static 全局属性

>

	使用Static关键字声明的属性，为全局属性

	使用Static关键字声明的属性，可以直接通过类名调用

	使用Static方法的时候，只能访问使用Static关键字声明的属性

	静态变量可以通过类名调用

	静态在实例化之前就会被调用, 与实例化无关(NEW)

	使用Static声明的方法只能访问Static声明的变量与方法

>

	class Home {
	    public  static String name = "World";
	    public String addr;
	    public String phone;
	    public void print() {
		System.out.println("Hello");
	    }
	    public static void println() {
		System.out.println(name);
	    }
	}

	public class Demo {
	    
	    public void main() {
		System.out.println(Home.name);
		Home.name = "BeiJing";
		System.out.println(Home.name);
		Home.name = "China";
		System.out.println(Home.name);
	    }
	    
	}

