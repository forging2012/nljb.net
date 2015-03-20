---
title: Android之监听返回键用法
date: '2015-03-10'
description:
categories:

tags:android

---

>

主要是实现onKeyDown函数，并且判断KeyEvent.KEYCODE_BACK即可

>

	    @Override
	    public boolean onKeyDown(int keyCode, KeyEvent event)
	    {
		if (keyCode == KeyEvent.KEYCODE_BACK )
		{
		    // 创建退出对话框
		    final AlertDialog.Builder isExit = new AlertDialog.Builder(this)
		    // 设置对话框标题
		    .setTitle("系统提示")
		    // 设置对话框消息
		    .setMessage("确定要退出吗?");
		    // 添加一个按钮，并监听按钮事件 (积极的;确实的)
		    isExit.setPositiveButton("确定", new DialogInterface.OnClickListener() {
			@Override
			public void onClick(DialogInterface dialog, int which) {
			    // 关闭, 自定义 exitActivity() 方法
	                    AgentApplication.exitActivity();
			}
		    });
		    // 添加一个按钮，并监听按钮事件 (消极的;否认的)
		    isExit.setNegativeButton("取消", new DialogInterface.OnClickListener() {
			@Override
			public void onClick(DialogInterface dialog, int which) {
			    // 关闭
			    return;
			}
		    });
		    // 显示对话框
		    isExit.create().show();
		}

		return false;

	    }

--- 

>

完美关闭程序，需要记录所有 Activity 统一关闭

>

	// 保存着所有 Activity 关闭时，统一释放
	public class AgentApplication extends Application {

	    private static List<Activity> activities = new ArrayList<Activity>();

	    public static void addActivity(Activity activity) {
		activities.add(activity);
	    }

	    public static void exitActivity() {
		for (Activity activity : activities) {
		    activity.finish();
		}
		System.exit(0);
	    }

	}

	// 一个 Activity
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		// 这里，每个 Activity onCreate 都要添加
		AgentApplication.addActivity(this);
	    }
	}

---

>

### OnkeyDown 和 OnBackPressed 介绍

>

	// OnkeyDown事件和OnkeyUp事件是不同事件。 
	// OnBackPressed方法会处理返回键的操作，不会向上传播
	// 如果你想向上传播 请使用OnkeyDown事件或OnkeyUp事件
	@Override
	public void onBackPressed() {
		super.onBackPressed();
	}

	// ...
	@Override
	public boolean onKeyDown(int keyCode, KeyEvent event) {
		return false;
	}


