---
title: Android之Toast简单使用方法
date: '2015-03-06'
description:
categories:

tags:android

---

Toast是Android中用来显示显示信息的一种机制，和Dialog不一样的是

Toast是没有焦点的，而且Toast显示的时间有限，过一定的时间就会自动消失。

---

	// 直接创建一个Toast并且显示
	Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
	// 也可以创建后设置一些参数再显示
	Toast toast = Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT);
	// 短时间 Toast.LENGTH_SHORT 长时间 Toast.LENGTH_LONG
	// 当然，也可以具体设置显示的时间
	toast.setDuration(Toast.LENGTH_SHORT);
	// 也可以加入一个 View 显示
	View toast.setView(...);
	// 位置, 也可以指定显示位置，默认为屏幕下部居中位置
	toast.setGravity(Gravity.CENTER, 0 ,0);
	// 开始显示
	toast.show();

---

	// 增加图片显示
	toast = Toast.makeText(getApplicationContext(),
	"带图片的Toast", Toast.LENGTH_LONG);
	// 同上
	toast.setGravity(Gravity.CENTER, 0, 0);
	// 获取 Layout 
	LinearLayout toastView = (LinearLayout) toast.getView();
	// 创建 ImageView
	ImageView imageCodeProject = new ImageView(getApplicationContext());
	// 设置 icon
	imageCodeProject.setImageResource(R.drawable.icon);
	// 添加到 toast
	toastView.addView(imageCodeProject, 0);
	// 显示
	toast.show();

---

	LayoutInflater inflater = getLayoutInflater();
	View layout = inflater.inflate(R.layout.custom,
	(ViewGroup) findViewById(R.id.llToast));
	ImageView image = (ImageView) layout
	.findViewById(R.id.tvImageToast);
	image.setImageResource(R.drawable.icon);
	TextView title = (TextView) layout.findViewById(R.id.tvTitleToast);
	title.setText("Attention");
	TextView text = (TextView) layout.findViewById(R.id.tvTextToast);
	text.setText("完全自定义Toast");
	toast = new Toast(getApplicationContext());
	toast.setGravity(Gravity.RIGHT | Gravity.TOP, 12, 40);
	toast.setDuration(Toast.LENGTH_LONG);
	toast.setView(layout);
	toast.show();

---

	new Thread(new Runnable() {
	    public void run() {
	     showToast();
	    }
	   }).start();
