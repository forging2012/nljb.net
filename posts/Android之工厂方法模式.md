---
title: Android之工厂方法模式
date: '2016-02-04'
description:
categories:

tags:android

---

>

### 工厂方法模式

>

	定义一个用于创建对象的接口，让子类决定实例化哪个类

>

---

>

	// 产品类（抽象)
	public abstract class Product {
		// 产品类的抽象方法
		// 由具体的产品类去实现
		public abstract void method();
	}

	// 具体产品A
	public class ConcreteProductA extends Product {
		@Override
		public void method() {
			System.out.println("我是具体的产品A");
		}
	}

	// 具体产品B
	public class ConcreteProductB extends Product {
		@Override
		public void method() {
			System.out.println("我是具体的产品A");
		}
	}

>

---

>

	// 抽象工厂类
	public abstract class Factory {
		// 抽象工厂方法
		// 具体生产什么由子类去实现
		public abstract Product createProduct();
	}

	// 具体工厂类
	public Class ConcreteFactory extends Factory {
		@Override
		public Product createProduct() {
			return new ConcreteProductA();
		}
	}

>

---

>

	// 客户类
	public class Client {
		public static void main(String[] args) {
			// 创建具体工厂对象, 向上转换为抽象工厂对象
			Factory factory = new ConcreteFactory();
			// 通过抽象方法，调用，具体工厂的方法
			// 具体工厂方法，
			// 创建具体产品对象，向上转换为抽象产品对象
			Product p = factory.createProduct();
			// 抽象产品对象, 调用, 具体产品对象方法
			p.method();
		}
	}

>

---

>

	// 利用反射的方式更简洁地来生产具体产品对象
	public abstract class Factory {
		// 抽象工厂方法，具体生产什么由子类去实现
		public abstract <T extends Product> T createProduct(Class<T> clz);
	}

	// 对于具体的工厂类，则通过反射获取类的示例即可
	public class ConcreteFactiry extends Factory {
		@Overrid
		public <T extends Product> T createProduct(Class<T> clz) {
			Product p = null;
			try {
				p = (Product) Class.forName(clz.getName()).newInstance();
			} catch (Exception e) {
				e.printStackTrace();
			}
			return (T) p;
		}
	}

	// Client
	public class Client {
		public static void main(String[] args) {
			Factory factiry = new ConcreteFactory();
			Product p = factory.createProduct(ConcreteProductB.class);
			p.method();
		}
	}

>

---

>

	// 下面通过一个汽车的自检流程来理解工厂方法

	// 汽车，抽象工厂类
	public abstract class Factory {
		// 汽车的工厂方法
		public abstract <T extends Car> T createCar(Class<T> clz);
	}

	// 汽车，具体工厂类
	public class CarFactory extends Factory {
		@Overrid
		public <T extends Car> T createCar(Class<T> clz) {
			Car car = null;
			try {
				car = (Car) Class.forName(clz.getName()).newInstance();
			} catch (Exception e) {
				e.printStackTrace();
			}
			return (T) car;
		}
	}

	// 汽车抽象类
	public abstract class Car {
		// 启动
		public abstract void drive();
		// 巡航
		public abstract void selfNavigation();
	}

	// 汽车具体类
	public abstract class AudiCar extends Car {
		// ...
	}	

	// 奥迪汽车，具体类
	public class AudiQ3 extends AudiCar {
		@Override
		public void drive() {
			System.out.println("Q3 启动了");
		}
		@Override
		public void selfNavigation() {
			System.out.println("Q3 自动巡航");
		}	
	}

	// 奥迪汽车，具体类
	public class AudiQ5 extends AudiCar {
		@Override
		public void drive() {
			System.out.println("Q5 启动了");
		}
		@Override
		public void selfNavigation() {
			System.out.println("Q5 自动巡航");
		}		
	}

	// 奥迪汽车，具体类
	public class AudiQ7 extends AudiCar {
		@Override
		public void drive() {
			System.out.println("Q7 启动了");
		}
		@Override
		public void selfNavigation() {
			System.out.println("Q7 自动巡航");
		}		
	}

	// Client
	public class Client {
		public static void mian(String[] args) {
			Factory factory = new CarFactory();
			Car audiQ3 = factory.createCar(AudiQ3.class)
			audiQ3.drive();
			audiQ3.selfNavigation();
			Car audiQ5 = factory.createCar(AudiQ5.class)
			audiQ5.drive();
			audiQ5.selfNavigation();
			Car audiQ7 = factory.createCar(AudiQ7.class)
			audiQ7.drive();
			audiQ7.selfNavigation();
		}
	}	
	
