---
title: Java之抽象类与接口
date: '2015-04-22'
description:
categories:

tags:java

---

>

### final 完结器(最终)

>

final关键字在JAVA中被称为完结器，表示最终的意思.

final能声明，类，方法，属性

final声明的对象，不能够被继承

final声明的方法，不能够被重写

final声明的变量，会变成常量，不可以被修改

>

	// 不能够被继承的类
	final class Hello {}

	class Hello {
		// 不能够被继承的方法
		public final void print() {
		}
		// 被变成常量的变量
		final String HOME = "bj";
	}

---

>

### 抽象类和接口的区别

>

接口是公开的，里面不能有私有的方法或变量，是用于让别人使用的，而抽象类是可以有私有方法或私有变量的，

>

实现接口的一定要实现接口里定义的所有方法，而实现抽象类可以有选择地重写需要用到的方法

应用里，最顶级的是接口，然后是抽象类实现接口，最后才到具体类实现。

口可以实现多重继承，而一个类只能继承一个超类，但可以通过继承多个接口实现多重继承

>

---

>

### abstract class ClassName 抽象类

>

声明而未被实现的方法，必须使用abstract抽象关键字声明

抽象类不能直接实例化，需要子类进行实例化

抽象类被子类继承，子类(如果不是抽象类)，必需重写抽象类中的所有抽象方法

>

	abstract class Abs {
	    public void hello() {
		System.out.println("Hello");
	    }
	    // 抽象方法
	    public abstract void print();
	}

	class AbsDemo extends Abs {

	    // 子类必须复写抽象类中所有抽象方法
	    @Override
	    public void print() {
		System.out.println("World");
	    }

	}

	public class xxx {

	    public void main() {
		AbsDemo demo = new AbsDemo();
		demo.hello();
		demo.print();
	    }

	}

---

>

### interface InterfaceName 接口

>

接口内全部是由全局常量和公共抽象方法组成

>

	
	// 接口 1
	interface Inter {
	    public static final int TAG = 100;

	    public boolean print();
	}
	
	// 接口 2
	interface Inter2 {
	    public static final int TAG2 = 100;

	    public boolean println2();
	}

	// 接口 3 继承 接口 1 + 2
	interface Inter3 extends Inter, Inter2 {
	}

	// 类 B 实现接口 3 = 1 + 2
	class B implements Inter3 {

	    @Override
	    public boolean print() {
		System.out.print("Hello");
		return false;
	    }

	    @Override
	    public boolean println() {
		System.out.print("World");
		return false;
	    }
	}

	// 类 A 实现接口 1 + 2
	class A implements Inter, Inter2 {

	    @Override
	    public boolean print() {
		System.out.print("Hello");
		return false;
	    }

	    @Override
	    public boolean println() {
		System.out.print("World");
		return false;
	    }
	}
	*/

	public class xxx {

	    public void main() {
		A a = new A();
		// Inter.TAG;
		a.print();
		a.println();
		B b = new B();
		b.print();
		b.println();
	    }

	}


