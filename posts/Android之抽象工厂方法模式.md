---
title: Android之抽象工厂方法模式
date: '2016-02-04'
description:
categories:

tags:android

---

>

### 抽象工厂方法模式

>

	虽然抽象工厂方法模式的类繁多，但是，主要还是分为四类

	AbstractFactory 抽象工厂角色
		它声明了一组用于创建一种产品的方法，每一个方法对应一种产品
	ConcreteFactory 具体工厂角色
		它实现了在抽象工厂中定义的创建产品的方法，生成一组具体产品
		这些产品构成了一个产品种类，每一个产品都位于某个产品等级结构中
	AbstractProduct 抽象产品角色
		它为每种产品舍生命接口
	ConcreteProduct 具体产品角色
		它定义具体工厂生产的具体产品对象，实现抽象产品接口中声明业务方法

>

---

>

	// 抽象车厂类	
	public abstract class CarFactory {

		// 生产轮胎
		public abstract ITire createTire();

		// 生产发动机
		public abstract IEngine createEngine();

		// 生产制动系统
		public abstract IBrake createBrake();

	}	

>

	// 轮胎相关类
	public interface ITire {
		// 轮胎
		void tire();
	}

	// 普通轮胎
	public class NormalTire implements ITire {
		@Override
		public void tire() {
			System.out.println("普通轮胎");
		｝
	}

	// 越野轮胎
	public class SUVTire implements ITire {
		@Override
		public void tire() {
			System.out.println("越野轮胎");
		}
	}

>

	// 发动机相关类
	public interface IEngine {
		// 发动机
		void engine();
	}

	// 国产发动机
	public class DomesticEngine implements IEngine {
		@Override
		public void engine() {
			System.out.println("国产发动机");
		}
	}

	// 进口发动机
	public class ImportEngine implements IEngine {
		@Override
		public void engine() {
			System.out.println("进口发动机");
		}
	}

>

	// 制动系统相关类
	public interface IBrake {
		// 制动系统
		void brake();
	}

	// 普通制动
	public class NormalBrake implements IBrake {
		@Override
		public void brake() {
			System.out.println("普通制动");
		}
	}

	// 高级制动
	public class SeniorBrake implements IBrake {
		@Override
		public void brake() {
			System.out.println("高级制动");
		}
	}

>

	// Q3 工厂类
	public class Q3Factory entends CarFactory {
		@Override
		public ITire createTire() {
			// Q3 使用普通轮胎
			return new NormalTire();		
		}
		@Override
		public IEngine createEngine() {
			// Q3 使用普通发动机
			return new DomesticEngine();
		}
		@Override
		public IEngine createBrake() {
			// Q3 使用普通制动
			return new NormalBrake();
		}
	}

	// Q5 工厂类
	public class Q5Factory entends CarFactory {
		@Override
		public ITire createTire() {
			// Q5 使用越野轮胎
			return new SUVTire();		
		}
		@Override
		public IEngine createEngine() {
			// Q5 使用进口发动机
			return new ImportEngine();
		}
		@Override
		public IEngine createBrake() {
			// Q5 使用高级制动
			return new SeniorBrake();
		}
	}

>

	public class Client {
		public static void main(String[] args) {
			CarFctory factoryQ3 = new Q3Factory();
			factoryQ3.createTire().tire();
			factoryQ3.createEngine().engine();
			factoryQ3.createBrake().brake();
			CarFctory factoryQ5 = new Q5Factory();
			factoryQ5.createTire().tire();
			factoryQ5.createEngine().engine();
			factoryQ5.createBrake().brake();
		}
	}	

>
		
