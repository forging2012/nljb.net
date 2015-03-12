---
title: Android之使用Animation设置View动画
date: '2015-03-12'
description:
categories:

tags:android

---

>

### Animation 

>

	// 主要是向View对象设置动画效果
	// 比如 Button, TextView ... 等 
        AlphaAnimation 透明度动画效果
        RotateAnimation 旋转动画效果
        ScaleAnimation 缩放动画效果
        TranslateAnimation 位移动画效果
        AnimationSet 混合动画

>

	// 处理 Activity 的 View
	// 向 View 增加动画效果
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			// AlphaAnimation 透明度动画效果
			// AlphaAnimation aa = new AlphaAnimation(0, 1);
			// RotateAnimation 旋转动画效果
			// RotateAnimation aa = new RotateAnimation(0, 360);
			// ScaleAnimation 缩放动画效果
			// ScaleAnimation aa = new ScaleAnimation(0, 1, 0, 1);
			// TranslateAnimation 位移动画效果
			// TranslateAnimation aa = new TranslateAnimation(0, 200, 0, 200);
			// AnimationSet 混合动画
			// AnimationSet as = new AnimationSet(true);
			// AlphaAnimation aa = new AlphaAnimation(0, 1);
			// aa.setDuration(1000);
			// as.addAnimation(aa);
			// TranslateAnimation ab = new TranslateAnimation(0, 200, 0, 200);
			// ab.setDuration(1000);
			// as.addAnimation(ab);
			// 持续时间
			// as.setDuration(1000);
			TranslateAnimation ab = new TranslateAnimation(0, 200, 0, 200);
			ab.setDuration(1000);
			// 监听动画
			ab.setAnimationListener(new Animation.AnimationListener() {
			    @Override
			    public void onAnimationStart(Animation animation) {
				// 动画开始
			    }

			    @Override
			    public void onAnimationEnd(Animation animation) {
				// 动画结束
			    }

			    @Override
			    public void onAnimationRepeat(Animation animation) {
				// ...
			    }
			});
			// 为View设置开始动画
                        // v.startAnimation(as);
		    }
		});
	}


---

>

### LayoutAnimationController

>

	// 布局动画效果
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		// 获取 View
		LinearLayout v = (LinearLayout) this.getLayoutInflater().inflate(R.layout.activity_main, null);
		// 创建动画
		ScaleAnimation sa = new ScaleAnimation(0, 1, 0, 1);
		sa.setDuration(5000);
		// 创建 Layout 动画
		LayoutAnimationController lac = new LayoutAnimationController(sa, 0.5f);
		// LayoutAnimationController.ORDER_NORMAL  正常顺序
		// LayoutAnimationController.ORDER_RANDOM  随机顺序
		// LayoutAnimationController.ORDER_REVERSE 倒序顺序
		lac.setOrder(LayoutAnimationController.ORDER_NORMAL);
		// 将 Layout 动画设置到 View
		v.setLayoutAnimation(lac);
		// 将 View 设置到 Activity
		setContentView(v);
	    }
	}

---

>

### 默认的布局转换动画

>

	// 如果你要使用默认的动画
	// 一个非常简单的方式是在View的XML布局文件中把android:animateLayoutchanges属性设置为true。
　	// 这样就自动地按照默认方式来对要移除或添加的View，还有Group中的其他View进行动画。
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="vertical" >

	    <Button
		android:id="@+id/addNewButton"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="Add Button" />
	    <!--
	    <GridLayout
		android:columnCount="4"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:id="@+id/gridContainer"
		// 默认的布局转换动画
		android:animateLayoutChanges="true"
		/>
	    -->

	    <LinearLayout
		android:id="@+id/gridContainer"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		// 默认的布局转换动画
		android:animateLayoutChanges="true" 
		android:orientation="vertical">
	    </LinearLayout>

	</LinearLayout>


