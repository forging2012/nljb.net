---
title: Android之自由扩展你的项目-Builder模式
date: '2016-02-03'
description:
categories:

tags:android

---

>

### Builder 模式

>

	将一个复杂对象的构建与它的表示分离
	使得同样的构建过程可以创建不同表示

>

	（1）相同的方法，不同的执行顺序，产生不同的事件结果
	（2）多个部件或零件，都可以装配到一个对象中
			但是产生的运行结果又不相同
	（3）产品类非常复杂，或者产品类中的调用顺序不同产生了不同的作用
			这个时候使用建造者模式非常适合
	（3）当初始化一个对象特别复杂，如参数多，且很多参数都具有默认值时

>

---

>

	// 通过Builder模式讲解计算机组装过


	// 计算机（抽象类）
	public abstract class Computer {

		private String mCPU;

		private String mDisplay;

		private String mOS;

		public void setCPU(String cpu) {
			mCPU = cpu;
		}

		public void setDisplay(String display) {
			mDisplay = display;
		}

		// 之所以用抽象方法，是因为
		// CPU 与 显示器 是可变的
		// 但是对于 Macbook 或者 Thinkpad 
		// TA们的系统已经决定是Mac OS 还是 WinX
		public abstract setOS(String os);

		@Override
		public String toString() {
			return String.format("CPU: %s , Display: %s , OS: %s");
		}

	}

	// 具体的 Computer 类, MacBook
	public class Macbook extends Computer {

		@Override
		public void setOS() {
			mOS = "Mac OS X 10.10";
		}
	
	}

	// 抽象 Builder 类型
	public abstract class Builder {
		// 设置 CPU
		public abstract void buildCPU(String cpu);
		// 设置 显示器
		public abstract void buildDisplay(String display);
		// 设置 系统
		public abstract void buildOS(String os);
		// 创建
		public abstract Computer create();
	}

	// 具体 Builder 类, MacbookBuilder
	public class MacbookBuilder extends Builder {

		private Computer mComputer = new Macbook();

		@Override
		public void buildCPU(String cpu) {
			mComputer.setCPU(cpu);	
		}

		@Override
		public void buildDisplay(String display) {
			mComputer.setDisplay(display);
		}

		@Override
		public void buildOS(String os) {
			mComputer.setOS();
		}

		@Override
		public void create() {
			return mComputer;
		}

	}

	// Director 类, 负责构造 Computer
	public class Director {

		Builder mBuilder = null;

		public Director(Builder builder) {
			mBuilder = builder;	
		}

		public void construce(String cpu, String display) {
			mBuilder.buildCPU(cpu);
			mBuilder.buildDisplay(display);
			mBuilder.buildOS();
		}	

	}

>

	// 测试
	Builder builder = new MacbookBuilder();
	Director director = new Director(builder);
	director.construce("英特尔-I7", "Retina-显示器");
	builder.create().toString();
	
>
