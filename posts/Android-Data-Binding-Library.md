---
title: Android Data Binding Library
date: '2015-07-23'
description:
categories:

tags:android

---

>

### Android Data Binding Library

>

	Data Binding即数据绑定,Data Binding 库实现在布局文件中实现数据绑定申明
	使数据的变化引起视图的自动更新，减少了逻辑代码，在Android中可以很方便的实现MVVM的开发模式。

>

了解MVVM之前，我们先简单说一下MVC、MVP模式。

>

	MVC是Model(模型)---View(视图)---(Controller)控制器的缩写
	它用一种将业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面
	在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。
	将系统进行MVC分层的核心思路就是分离组件，降低组件耦合性，组件独立演化。

>

	MVP是Model--View--Presenter的缩写，MVP与MVC的主要区别是在MVP中View并不直接使用Model
	它们之间的通信是通过Presenter (MVC中的Controller)来进行的，所有的交互都发生在Presenter内部
	而在MVC中View会从直接Model中读取数据而不是通过 Controller。
	模型与视图完全分离，我们可以修改视图而不影响模型，可以更高效地使用模型，因为所有的交互在Presenter里面。

>

	MVVM是Model-View-ViewModel的缩写,Model提供数据、View负责显示、ViewModel负责逻辑的处理
	与MVP的区别是，ViewModel与View之间采用双向绑定（data-binding）：
		View的变动，自动反映在 ViewModel，反之ViewModel的变化自动引起View的改变 。
	ViewModel作为View的数据映射，通常View上有什么属性，ViewModel上也会存在相应的一个属性
	这两个属性通过事件实现了双向的绑定，Data Binding Library 替我们完成了这样的绑定过程。 

>

	1. 低耦合: 视图（View）可以独立于Model变化和修改
		一个ViewModel可以绑定到不同的"View"上，当View变 化的时候Model可以不变，当Model变化的时候View也可以不变。
	2. 可重用性: 你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
	3. 可测试: 界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。

>

---

>

### 环境配置

>

	Data Binding 插件需要Gradle 1.3以上及Android Studio 1.3.

	配置Guide文件：添加Data binding 包路径

	buildscript {  
		repositories {  
			jcenter()  
		}  
		dependencies {  
			classpath "com.android.tools.build:gradle:1.3.0-beta1"  
			classpath "com.android.databinding:dataBinder:1.0-rc0"  
	  
		}  
	} 

	然后确保jcenter是在子项目的库列表中       

	allprojects {
	   repositories {
		jcenter()
	   }
	}

	在android插件后面添加dataBinding插件
	apply plugin: ‘com.android.application'
	apply plugin: 'com.android.databinding' 

>

---

>

### 如何使用

>

	// activity_main.xml
	<layout xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:bind="http://schemas.android.com/apk/res-auto">
		<data class="com.jikexueyuan.databinding.JiKeUserBinding">
			<import type="android.view.View"/>
			<import type="java.util.List"/>
			<variable
				name="user"
				type="com.jikexueyuan.jikedatabinding.JikeUser"/>
			<variable
				name="sex"
				type="String"/>
			<variable
				name="list"
				type="List&lt;String>"/>

		</data>
		<LinearLayout
			android:layout_width="match_parent"
			android:layout_height="match_parent"
			android:orientation="vertical">

			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="@{user.name}" />

			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="Iwen是男人"
				android:visibility="@{user.isMan==0?View.VISIBLE:View.GONE}"/>

			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="@{sex}" />
			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="@{list[1]}" />
			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="@{user.isMan==0?@string/man:@string/woman}"
				/>
			<include
				layout="@layout/phone_layout"
				bind:user="@{user}"/>
		</LinearLayout>
	</layout>

	// phone_layout.xml
	<?xml version="1.0" encoding="utf-8"?>
	<layout>
		<data>
			<variable
				name="user"
				type="com.jikexueyuan.jikedatabinding.JikeUser"/>
		</data>
		<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
			android:layout_width="match_parent"
			android:layout_height="match_parent">
			<TextView
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="@{user.phone}"/>
		</LinearLayout>
	</layout>

>

	// MainActivity.java
	public class MainActivity extends AppCompatActivity {

		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			//setContentView(R.layout.activity_main);
			JiKeUserBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
			JikeUser user = new JikeUser();
			user.name = "Iwen";
			user.phone = "110";
			user.isMan=1;

			binding.setSex("男");
			binding.setUser(user);

			List<String> mlist=new ArrayList<>();
			mlist.add("eoeAndroid");
			mlist.add("极客学院");
			mlist.add("Iwen");

			binding.setList(mlist);

		}

	}

	// JikeUser.java
	public class JikeUser {
		public String name;
		public String phone;
		public int isMan;

	}


