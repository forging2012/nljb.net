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

	也就是说，非静态方法可以访问静态属性的变量或方法

	但是，静态方法无法调用，非静态的变量或方法

	理解：因为静态变量或方法是在实例化之前的

	也就是在实力后的方法中调用静态变量或者方法是没问题的

	因为TA们很早之前就被实例化了。

	但是为什么静态方法不能调用非静态的变量或者方法呢

	因为静态方法在实例化的时候，非晶态方法还没出生呢...

>

	class Home {
	    public  static String name = "World";
	    public String addr;
	    public String phone;
	    public void print() {
		System.out.println("Hello");
	    }
	    public static void println() {
		// 这样是可以的, 因为TA与本函数一起存在
		System.out.println(name);
		// 这样是不可以的, 还没出生
		System.out.println(addr);
		// 这样是不可以的, 因为TA还没出生呢
		baidu();
	    }
	    public void baidu() {
		// 这样是可以的, 因为TA早就存在了
		println()
	    };
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

