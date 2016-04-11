---
title: Android之ActionMode的使用
date: '2015-07-20'
description:
categories:

tags:android

---

>

Android的menu有多种实现方式，这里介绍一种新的menu实现方式：ActionMode。

>

ActionMode是Android 3.0以后出现的，我们可以使用AppCompat库使ActionMode兼容至Android 2.1。

>

Android 3.0以前，我们处理列表的长按事件经常使用Context Menu

>

Android3.0以后，我们有了新的选择：ActionMode。下图左边效果为Context Menu右边效果为ActionMode。

>

<img src="{{urls.media}}/Android之ActionMode的使用/1.jpg" alt="" width="350" hight="420" >
<img src="{{urls.media}}/Android之ActionMode的使用/2.jpg" alt="" width="350" hight="420" >

>

---

>

Android开发者应该都熟悉Context Menu了，Context Menu是悬浮在操作项之上的视图。

>

ActionMode是临时占据了ActionBar的位置。下面给出ActionMode的实现方法。

>

	// 使用ActionMode需要两步骤：

>

---

>

1、实现ActionMode.Callback接口，并处理ActionMode的生命周期，ActionMode的生命周期如下图:

>

	private ActionMode.Callback mCallback = new ActionMode.Callback() {  

			@Override  
			public boolean onPrepareActionMode(ActionMode mode, Menu menu) {  
				return false;  
			}  

			@Override  
			public void onDestroyActionMode(ActionMode mode) {  
				// TODO Auto-generated method stub  
			}  

			@Override  
			public boolean onCreateActionMode(ActionMode mode, Menu menu) {  
				MenuInflater inflater = mode.getMenuInflater();  
				inflater.inflate(R.menu.actionmode, menu);  
				return true;  
			}
	  
			@Override  
			public boolean onActionItemClicked(ActionMode mode, MenuItem item) {  
				boolean ret = false;  
				if (item.getItemId() == R.id.actionmode_cancel) {  
					mode.finish();  
					ret = true;  
				}  
				return ret;  
			} 
 
	}; 

>

---

>

2、事件触发时，调用startActionMode()方法。

>

	someView.setOnLongClickListener(new View.OnLongClickListener() {  
		// Called when the user long-clicks on someView  
		public boolean onLongClick(View view) {  
			if (mActionMode != null) {  
				return false;  
			}  
		 
			// Start the CAB using the ActionMode.Callback defined above  
			mActionMode = getActivity().startActionMode(mActionModeCallback);  
			view.setSelected(true);  
			return true;  
		}  

	});  

