---
title: Android之注解式框架及Butterknife的使用
date: '2015-06-24'
description:
categories:

tags:android

---

>

### 注解式框架

>

### 什么是注解式开发:

	JDK1.5后支持注解方式,想用注解式开发,就要自定义注解

	结构:
		@+注解名(也可以叫类名)+传递的属性值,key和value,
		可设置目标范围:方法(Method)、属性(Filed)、类(Type)

	自定义注解要用到
		@interface:用于定义注解;
		@Target:用于描述注解的使用范围;
		@Retention: 注解的生命周期,一般RetentionPolicy.RUNTIME

	在Android中使用一般是简化代码,提升开发效率,清晰简介

>

---

>

### 流的注解式框架有Dagger、ButerKnife、AndroidAnnotations。 

	AndroidAnnotations是一个利用注解方式来简化代码结构,提高开发效率的开源框架。
		一，配置麻烦,需要在项目清单里注册生成的子类。
		二，反射机制会占用资源内存和耗时。

	ButerKnife用起来方便,配置简单,强大的View注入绑定和简单的常用方法注解。

	Dagger采用预编译技术,高效,但是对View绑定操作注解不是很方便。

>

---

>

### ButerKnife特点: 

	强大方便的处理View绑定和Click事件,简化代码,提升开发效率

	方便的处理ListView的Adapter里的ViewHolder绑定问题 

	运行时不会影响APP效率,使用配置方便

	代码思路清晰,可读性强

>

---

>

### View绑定:

	onCreate里注册:
		ButerKnife.inject(this);

	Activity声明绑定控件,例如:
		@InjectView(R.id.title)
		TextView title;

	onclick等事件处理: 例如:
		@OnClick(R.id.submit) 
		public void sayHi(Button button){
			button.setText("Hello!");	
		}

>

---

>

	@InjectView(R.id.arc_progress)
	ArcProgress arcProgress;

	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// Init
		ButterKnife.inject(this);
	}

	@OnClick(R.id.arc_progress)
	public void initProgress() {
		// ...
	}

>

---

>

用ListView展示一个列表数据，每个Item里含有一个Button，可以点击

>

	// @OnItemSelected
	// @OnItemClick

	@Override
	public View getView(final int position, View convertView, ViewGroup parent) {
		ConfigViewHolder configViewHolder = null;
		if (convertView == null) {
		    convertView = View.inflate(context, R.layout.adapter_config, null);
		    configViewHolder = new ConfigViewHolder(convertView);
		    convertView.setTag(configViewHolder);
		} else {
		    configViewHolder = (ConfigViewHolder) convertView.getTag();
		}
		......
	} 

	// ViewHolder
	class ConfigViewHolder {

		@InjectView(R.id.item_status)
		ImageView item_status;

		@InjectView(R.id.item_description)
		TextView item_description;

		public ConfigViewHolder(View view) {
		    ButterKnife.inject(this, view);
		}

	}

