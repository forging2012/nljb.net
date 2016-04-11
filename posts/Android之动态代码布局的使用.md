---
title: Android之动态代码布局的使用
date: '2015-05-06'
description:
categories:

tags:android

---

>

### 动态代码布局

>

所谓的动态代码布局，也就是不使用XML布局(或不完全使用XML布局)

>

---

>

	    // 动态代码布局
	    @Override
	    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {

		// 定义了一个 MySwipeRefreshLayout 的 Layout 
		layout = new MySwipeRefreshLayout(getActivity());

		// 设置ID 
		layout.setId(this.generateViewId());
	
		// 设置 LayoutParams
		ViewGroup.LayoutParams layoutParams = new ViewGroup.LayoutParams(
			ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
		layout.setLayoutParams(layoutParams);

		// 下拉更新
		layout.setColorScheme(android.R.color.holo_blue_dark,
			android.R.color.holo_blue_light,
			android.R.color.holo_green_light,
			android.R.color.holo_green_light);

		return super.onCreateView(inflater, container, savedInstanceState);

	    }

	    // replace Fragment
	    // 这里是个 Fragment 替换函数，想要替换 Fragment 就需要 View 的 ID
	    // 所以上面使用了 generateViewId() 获取一个 ID 并且设置给 Fragment
	    // 这里使用的时候只需要 Fragment.getID() 就可以了
	    public void replaceFragment(int viewId, android.app.Fragment fragment) {
		FragmentTransaction transaction = getFragmentManager().beginTransaction();
		transaction.replace(viewId, fragment);
		transaction.commit();
	    }

	    /**
	     * An {@code int} value that may be updated atomically.
	     */
	    private static final AtomicInteger sNextGeneratedId = new AtomicInteger(1);

	    /**
	     * 动态生成View ID
	     * API LEVEL 17 以上View.generateViewId()生成
	     * API LEVEL 17 以下需要手动生成
	     */
	    public static int generateViewId() {

		if (Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN_MR1) {
		    for (; ; ) {
			final int result = sNextGeneratedId.get();
			// aapt-generated IDs have the high byte nonzero; clamp to the range under that.
			int newValue = result + 1;
			if (newValue > 0x00FFFFFF) newValue = 1; // Roll over to 1, not 0.
			if (sNextGeneratedId.compareAndSet(result, newValue)) {
			    return result;
			}
		    }
		} else {
		    return View.generateViewId();
		}
	    }

>

---

>

	// 简单布局LinearLayout
	LinearLayout llayout = new LinearLayout(mContext);
	llayout.setOrientation(LinearLayout.VERTICAL);
	LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(
		LinearLayout.LayoutParams.MATCH_PARENT,
		LinearLayout.LayoutParams.MATCH_PARENT
	);
	llayout.setLayoutParams(layoutParams);

	Button btn = new Button(mContext);
	btn.setText("This is Button");
	btn.setPadding(8, 8, 8, 8);
	btn.setLayoutParams(lp);

	llayout.addView(btn);

	//这是在Activity的onCreate()中设置布局
	setContentView(llayout);

	btn.setOnClickListener(new View.OnClickListener() {
	    @Override
	    public void onClick(View v) {
		Toast.makeText(mContext,
		"This is dynamic activity", Toast.LENGTH_LONG).show();
	    }
	});

>

---

>

	// 复杂布局RelativeLayout 

	//父控件
	RelativeLayout myLayout = new RelativeLayout(this);
	myLayout.setBackgroundColor(Color.BLUE); 

	//两个子控件
	Button myButton = new Button(this);
	EditText myEditText = new EditText(this);

	//重点:生成对应的ID
	myButton.setId(generateViewId());
	myEditText.setId(generateViewId());

	//子控件位置
	RelativeLayout.LayoutParams buttonParams =
		new RelativeLayout.LayoutParams(
			RelativeLayout.LayoutParams.WRAP_CONTENT,
			RelativeLayout.LayoutParams.WRAP_CONTENT);
	buttonParams.addRule(RelativeLayout.CENTER_HORIZONTAL);
	buttonParams.addRule(RelativeLayout.CENTER_VERTICAL);

	RelativeLayout.LayoutParams textParams =
		new RelativeLayout.LayoutParams(
			RelativeLayout.LayoutParams.WRAP_CONTENT,
			RelativeLayout.LayoutParams.WRAP_CONTENT);
	textParams.addRule(RelativeLayout.CENTER_HORIZONTAL);
	textParams.setMargins(0, 0, 0, 80);
	//重点在这里
	textParams.addRule(RelativeLayout.ABOVE, myButton.getId());

	//添加布局
	myLayout.addView(myButton, buttonParams);
	myLayout.addView(myEditText, textParams);
	setContentView(myLayout);


