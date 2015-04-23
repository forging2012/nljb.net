---
title: Java面向对象之继承
date: '2015-04-23'
description:
categories:

tags:java

---

>

### 继承 (extends)

>

	扩展(子类)父类的功能

	不允许继承多个类, 但是可以多层继承

	子类不能直接访问父类的私有资源

	父类的构造方法先执行子类构造后执行

	重写需方法名相同，返回类型相同，参数相同

	被子类重写的方法不能拥有比父类更严格的访问权限

	private(当前类) < default(整个包) < public(可访问)

	super 强行调用父类的对象使用或方法执行

	重载，方法名称相同，参数的类型或个数不同
		发生在一个类中，对权限没有要求

	重写，方法名称，参数类型和个数，返回值类型，全部相同
		不能拥有比父类更加严格的权限，发生在继承中

>

	class Person {

	    public Person() {
		System.out.println("父类构造先执行");
	    }

	    public void tell() {

	    }

	    // 公共
	    public String name;

	    // 公共
	    public String age;

	    public String getAge() {
		return age;
	    }

	    public String getName() {
		return name;
	    }

	}

	class JD extends Person {

	    public JD() {
		System.out.println("子类构造后执行");
	    }

	    // 重载
	    public void Print() {

	    }
	    
	    // 重载
	    public void Print(String name) {

	    }

	    // 继承了父类的公共对象
	    // ...

	    // 私有，不可以子类访问
	    private String phone;

	    public String getPhone() {
		return phone;
	    }

	    // 子类可以直接修改父类的公共对象
	    public void reSetName(String name) {
		// super 关键字作用
		super.name = name;
	    }

	    // 重写方法
	    public void tell() {
		// 如果不强制调用父类重写方法，默认是不会调用的
		// 强制调用父类方法
		super.tell();
		System.out.println("重写父类方法");
	    }

	}

	public class Demo {

	    public void main() {
		JD jd = new JD();
		jd.getAge();
		jd.getName();
		jd.getPhone();
	    }

	}


