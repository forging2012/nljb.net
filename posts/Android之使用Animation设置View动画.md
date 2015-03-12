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

	// 可以向所有的View设置动画效果
        AlphaAnimation 透明度动画效果
        RotateAnimation 旋转动画效果
        ScaleAnimation 缩放动画效果
        TranslateAnimation 位移动画效果
        AnimationSet 混合动画

>

	// 处理 Activity 的 View
	// 向 View 增加动画效果
	public class Main2Activity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		// 从 Layout 获取 View
		View v = this.getLayoutInflater().inflate(R.layout.activity_main2, null);
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
		// 为View设置开始动画
		// v.startAnimation(as);
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
		// 设置 View
		setContentView(v);
	    }

	}

