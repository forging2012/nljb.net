---
title: Android之动态生成View的ID
date: '2015-06-16'
description:
categories:

tags:android

---

>

### 动态生成View-ID

>

	为什么需要ID, 因为在replaceFragment等操作时需要使用ID
	// 设置的ID与R.id.xxx相同，代表这个View的唯一标识

>

        // 初始化
        layout = new MySwipeRefreshLayout(getActivity());
	// 这里需要设置一个ID给该View对象
        layout.setId(this.generateViewId());
        ViewGroup.LayoutParams layoutParams = new ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        layout.setLayoutParams(layoutParams);
        // 下拉更新
        layout.setColorSchemeResources(android.R.color.holo_blue_dark,
                android.R.color.holo_blue_light,
                android.R.color.holo_green_light,
                android.R.color.holo_green_light);

>

	// replace Fragment
	public void replaceFragment(int viewId, android.app.Fragment fragment) {
		FragmentTransaction transaction = getFragmentManager().beginTransaction();
		transaction.replace(viewId, fragment);
		transaction.commit();
	}

>
	
---


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


