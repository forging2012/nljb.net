---
title: Android之在Layout中自定义View
date: '2015-04-28'
description:
categories:

tags:android

---

>

### 在Layout中自定义View

>

经常会看到在XML文件中调用别人的View就可以显示出各种奇妙的页面

>

简单的学习了一下，下面说一下如何自定义一个View, 并设置背景色

>

	// 第一步，创建一个继承自View的类
	public class MyView extends View {

	    //  背景颜色
	    private int background;

	    // 默认背景颜色
	    private final int default_background = Color.rgb(66, 145, 241);

	    // 构造
	    public MyView(Context context) {
		// 这里确保每一级都会被触发
		this(context, null);
	    }

	    // 构造
	    public MyView(Context context, AttributeSet attrs) {
		// 这里确保每一级都会被触发
		this(context, attrs, R.attr.MyViewStyle);
	    }

	    // 构造
	    public MyView(Context context, AttributeSet attrs, int defStyle) {
		// 执行父类构造
		super(context, attrs, defStyle);
		// 初始化
		final TypedArray attributes = context.getTheme().obtainStyledAttributes(attrs, R.styleable.MyView, defStyle, 0);
		// 获取设置的背景颜色
		background = attributes.getColor(R.styleable.MyView_background, default_background);
		// 设置
		this.setBackgroundColor(background);
	    }

	}

---

	// 第二步，在XML-Layout中使用
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:background="@drawable/guide_0"
	    android:orientation="vertical">

	    // 这里使用的就是自定义的View
	    <com.example.nljb.surpass.MyView
		// 可以使用自定义的View的设置参数
		app:background="#ffff5633"
		// 可以使用继承自View的设置参数
		android:layout_width="match_parent"
		android:layout_height="50dp"/>

	</RelativeLayout>	

---

	// 第三步，自定义View的参数(第一步已经讲了如何使用)
	// values/attrs
	<?xml version="1.0" encoding="utf-8"?>
	<resources>

	    <declare-styleable name="MyView">
		// format 类型有很多 ...
		<attr name="background" format="color"/>
        	// <attr name="..." format="integer"/>
        	// <attr name="..." format="dimension"/>
        	// <attr name="..." format="enum">
            	//	<enum name="..." value="0"/>
            	//	<enum name="..." value="1"/>
        	// </attr>
		// <attr name="..." format="string"/>
		// <attr name="..." format="boolean"/>
		...
	    </declare-styleable>

	    <declare-styleable name="Themes">
		<attr name="MyViewStyle" format="reference"/>
	    </declare-styleable>
	</resources>


