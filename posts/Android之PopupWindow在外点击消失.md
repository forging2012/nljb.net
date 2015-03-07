---
title: Android之PopupWindow在外点击消失
date: '2015-03-06'
description:
categories:

tags:android

---

### PopupWindow 如果要想点击其他地方使其隐藏

>

	// PopupWindow
	// 载入一个Layout视图
	View root = this.getLayoutInflater().inflate(R.layout.window, null);

	// 通过视图及布局生成 PopupWindow
	// import android.view.ViewGroup.LayoutParams;
	// import android.widget.PopupWindow;
        final PopupWindow popup = new PopupWindow(root, LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT, true);

	// 使其聚焦 
	mPopupWindow.setFocusable(true); 

	// 设置允许在外点击消失 
	mPopupWindow.setOutsideTouchable(true); 

	//刷新状态 
	mPopupWindow.update(); 

	//点back键和其他地方使其消失,设置了这个才能触发OnDismisslistener ，设置其他控件变化等操作 
	// 设置背景色 ...
	mPopupWindow.setBackgroundDrawable(new BitmapDrawable()); 

	// new BitmapDrawable() 已经过时，可以使用 new BitmapDrawable(this.getResources())
	// 背景色的设置还可以
	// popup.setBackgroundDrawable(new ColorDrawable(0x55000000));

	// 监听按钮事件，来触发显示
	popup.showAtLocation(findViewById(R.id.button), Gravity.CENTER, 20, 20);

---

### 关于 PopupWindow 其它区域变暗效果

>

网上查了一下，没有什么具体的好办法，有设置背景色的，有调节透明度的

>

	// 如果这样，默认非显示区域都是透明的,但是全屏的缘故，在外点击消失失效
	PopupWindow(root, LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT, true);

	// 设置背景色，..... 有点滥
	popup.setBackgroundDrawable(new ColorDrawable(0x55000000));
	
	// 透明度，靠谱点，将主窗口透明
	// 记得关闭窗口的时候要设置回来哦!
	WindowManager.LayoutParams lp = getWindow().getAttributes();
	lp.alpha = 0.3f;
	getWindow().setAttributes(lp);

	// 具体介绍
	// alpha 渐变透明度动画效果
	0.0 表示完全透明
	1.0 表示完全不透明
	以上值取0.0-1.0之间的float数据类型的数字

	// 返回时,恢复透明度
        popup.setOnDismissListener(new PopupWindow.OnDismissListener() {
            @Override
            public void onDismiss() {
                WindowManager.LayoutParams lp = getWindow().getAttributes();
                lp.alpha = 1.0f;
                getWindow().setAttributes(lp);
            }
        });

	// 命令关闭 
	popup.dismiss();

