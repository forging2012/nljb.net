---
title: Android之PreferenceFragment使用方法
date: '2015-03-18'
description:
categories:

tags:android

---

>

### PreferenceFragment

>

Android应用程序通常要提供首选项，以允许用户定制应用程序。

例如，可以允许用户保存那些用于访问Web资源的登录凭据, 等等。

在Android中，可以使用PreferenceActivity基类为用户显示一个用于编辑首选项的活动。

在Android 3.0和更高版本中，可以使用PreferenceFragment类实现相同的功能。

>

	// XML
	// 新建 (res/xml/preferences.xml)
	<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android" >
	    <CheckBoxPreference
		android:key="child_checkbox_preference"
		android:summary="这是一个可见的子类"
		android:title="子类复选框首选项"
		android:summaryOn="已开启"
		android:summaryOff="已关闭"
		android:onClick="onClick"/>
	</PreferenceScreen>  	

	// Activit
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
                // 替换 Fragment
		getFragmentManager().beginTransaction()
                .replace(R.id.xxx, new MyPreference())
                .commit();
	    }

	  // PreferenceFragment
	  class MyPreference extends PreferenceFragment {

		// 监听 Preference Click
		@Override
		public boolean onPreferenceTreeClick(PreferenceScreen preferenceScreen, Preference preference) {
		    Log.i("INFO", preference.getKey());
		    // 也可以通过 KEY 获取
		    // findPreference("child_checkbox_preference")
		    if (preference.getKey().equals("child_checkbox_preference")) {
			CheckBoxPreference child_checkbox_preference = (CheckBoxPreference) preference;
			Toast.makeText(MainActivity.this, String.valueOf(child_checkbox_preference.isChecked()), Toast.LENGTH_SHORT).show();
		    }
		    return super.onPreferenceTreeClick(preferenceScreen, preference);
		}

		@Override
		public void onCreate(Bundle savedInstanceState) {
		    // TODO Auto-generated method stub
		    super.onCreate(savedInstanceState);
		    // 添加 Preferences XML
		    addPreferencesFromResource(R.xml.preferences);
		    // 选项监听 ...
		    // findPreference("child_checkbox_preference").setOnPreferenceClickListener(...);
		    // 选项监听 当 Preference 的值发生改变时触发该事件，true则以新值更新控件的状态，false 则 不保存
		    findPreference("child_checkbox_preference").setOnPreferenceChangeListener(new Preference.OnPreferenceChangeListener() {
			@Override
			public boolean onPreferenceChange(Preference preference, Object newValue) {
			    // 返回 false 修改不会生效
			    return false;
			}
		    });
		    /*
		    // 获取 Preferences Manager
		    PreferenceManager manager = getPreferenceManager();
		    // 获取 选项状态
		    CheckBoxPreference child_checkbox_preference = (CheckBoxPreference) manager.findPreference("child_checkbox_preference");
		    // ...
		    Toast.makeText(MainActivity.this, String.valueOf(child_checkbox_preference.isChecked()), Toast.LENGTH_SHORT).show();
		    */
		}
		public void onClick() {
		    Toast.makeText(MainActivity.this, "成功", Toast.LENGTH_SHORT).show();
		}
	    }
	}



