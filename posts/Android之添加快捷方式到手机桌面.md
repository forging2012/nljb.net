---
title: Android之添加快捷方式到手机桌面
date: '2015-07-06'
description:
categories:

tags:android

---

>

### 添加快捷方式

>

---

>

### 权限

>

要在手机桌面上添加快捷方式，首先需要在manifest中添加权限。

>

	<!-- 添加快捷方式 -->
	<uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" />
	<!-- 移除快捷方式 -->
	<uses-permission android:name="com.android.launcher.permission.UNINSTALL_SHORTCUT" />
	<!-- 查询快捷方式 -->
	<uses-permission android:name="com.android.launcher.permission.READ_SETTINGS" />

>

---

>

### 添加快捷方式

>

添加快捷方式，是向桌面应用(launcher)发送相关action的广播，相关的action如下：

>

	public static final String ACTION_ADD_SHORTCUT = "com.android.launcher.action.INSTALL_SHORTCUT";

>

添加快捷方式：

>

	public static void addShortcut(Context context, int appName, int appIcon) {

		String ACTION_INSTALL_SHORTCUT = "com.android.launcher.action.INSTALL_SHORTCUT";

		// 快捷方式要启动的包
		Intent intent = new Intent(context, context.getClass());

		// 设置快捷方式的参数
		Intent shortcutIntent = new Intent(ACTION_INSTALL_SHORTCUT);

		// 设置名称
		shortcutIntent.putExtra(
				Intent.EXTRA_SHORTCUT_NAME, context.getResources().getString(appName)); // 设置启动 Intent
		shortcutIntent.putExtra(Intent.EXTRA_SHORTCUT_INTENT, intent);

		// 设置图标
		shortcutIntent.putExtra(Intent.EXTRA_SHORTCUT_ICON_RESOURCE,
				Intent.ShortcutIconResource.fromContext(context, appIcon));

		// 只创建一次快捷方式
		shortcutIntent.putExtra("duplicate", false);

		// 创建
		context.sendBroadcast(shortcutIntent);

	}

>

---

>

### 移除快捷方式

>

移除快捷方式的action：

>

	public static final String ACTION_REMOVE_SHORTCUT = "com.android.launcher.action.UNINSTALL_SHORTCUT";

>

移除快捷方式的方法：

>

	private void removeShortcut(String name) {
		// remove shortcut的方法在小米系统上不管用，在三星上可以移除
		Intent intent = new Intent(ACTION_REMOVE_SHORTCUT);

		// 名字
		intent.putExtra(Intent.EXTRA_SHORTCUT_NAME, name);

		// 设置关联程序
		Intent launcherIntent = new Intent(MainActivity.this,
				MainActivity.class).setAction(Intent.ACTION_MAIN);

		intent.putExtra(Intent.EXTRA_SHORTCUT_INTENT, launcherIntent);

		// 发送广播
		sendBroadcast(intent);
	}

>

---

>

### 查询快捷方式

>

查询快捷方式是否存在的方法是从网上其他资料那里查来的，但是测试查询的时候失败了，两个手机(小米、三星)都查不到。

>

	private boolean hasInstallShortcut(String name) {

		boolean hasInstall = false;

		final String AUTHORITY = "com.android.launcher2.settings";
		Uri CONTENT_URI = Uri.parse("content://" + AUTHORITY
				+ "/favorites?notify=true");

		// 这里总是failed to find provider info
		// com.android.launcher2.settings和com.android.launcher.settings都不行
		Cursor cursor = this.getContentResolver().query(CONTENT_URI,
				new String[] { "title", "iconResource" }, "title=?",
				new String[] { name }, null);

		if (cursor != null && cursor.getCount() > 0) {
			hasInstall = true;
		}

		return hasInstall;

	}

>

---

>

转自: http://www.cnblogs.com/mengdd/p/3837592.html (感谢)

>

