---
title: Android之Layout实现Animation动画
date: '2015-11-02'
description:
categories:

tags:android

---

>

### Layout实现Animation动画

>

---

	// 获取Layout
	LinearLayout head = (LinearLayout) findViewById(R.id.head);

	// 设置Layout的宽高
	ViewGroup.LayoutParams lp = head.getLayoutParams();
	if (lp == null) {
		lp = new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
	}
	int widthMeasureSpec = ViewGroup.getChildMeasureSpec(0, 0, lp.width);
	int heightMeasureSpec;
	int heightTemp = lp.height;
	if (heightTemp > 0) {
		heightMeasureSpec = View.MeasureSpec.makeMeasureSpec(heightTemp, View.MeasureSpec.EXACTLY);
	} else {
		heightMeasureSpec = View.MeasureSpec.makeMeasureSpec(0, View.MeasureSpec.UNSPECIFIED);
	}
	head.measure(widthMeasureSpec, heightMeasureSpec);

	// 隐藏Layout
	RelativeLayout.LayoutParams params = (RelativeLayout.LayoutParams) head.getLayoutParams();
	params.setMargins(head.getLeft(), -head.getMeasuredHeight(), head.getRight(), head.getBottom());
	head.setLayoutParams(params);

	// 显示Layout动画
	Animation mTranslateAnimation = new TranslateAnimation(0, 0, 0, head.getMeasuredHeight());
	mTranslateAnimation.setDuration(1000);
	mTranslateAnimation.setAnimationListener(new Animation.AnimationListener() {
				public void onAnimationStart(Animation animation) {
				}

				public void onAnimationEnd(Animation animation) {
					head.clearAnimation();
					RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(head.getLayoutParams());
					// params.setMargins(0, head.getMeasuredHeight(), 0, 0);
					head.setLayoutParams(params);
				}

				public void onAnimationRepeat(Animation animation) {

				}
			});
	head.startAnimation(mTranslateAnimation);


