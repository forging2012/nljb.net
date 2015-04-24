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


---

>

<img src="{{urls.media}}/Android之PreferenceFragment使用方法/1.png" alt="" width="600">

>

	<?xml version="1.0" encoding="utf-8"?>  
	<PreferenceScreen  
		xmlns:android="http://schemas.android.com/apk/res/android">  
	  
	    <PreferenceCategory  
		    android:title="显示一排偏好">  
		<CheckBoxPreference  
			android:key="checkbox_preference"  
			android:title="开关偏好"  
			android:summary="这是一个开关按钮" />  
	    </PreferenceCategory>  
	    <PreferenceCategory  
		    android:title="基于对话框的偏好">  
		<EditTextPreference  
			android:key="edittext_preference"  
			android:title="文本输入偏好"  
			android:summary="使用一个文本框对话框"  
			android:dialogTitle="输入你的宠物" />  
		<ListPreference  
			android:key="list_preference"  
			android:title="列表偏好"  
			android:summary="使用一个列表对话框"  
			android:entries="@array/entries_list_preference"  
			android:entryValues="@array/entryvalues_list_preference"  
			android:dialogTitle="选择一个" />  
	    </PreferenceCategory>  
	    <PreferenceCategory  
		    android:title="启动偏好">  
		<PreferenceScreen  
			android:key="screen_preference"  
			android:title="屏幕"  
			android:summary="显示另一个偏好屏幕">  
		      
		    <!-- You can place more preferences here that will be shown on the next screen. -->  
			       
		    <CheckBoxPreference  
			    android:key="next_screen_checkbox_preference"  
			    android:title="开关偏好"  
			    android:summary="另一个屏幕上的偏好" />  
		</PreferenceScreen>  
	  
		<PreferenceScreen  
			android:title="意图偏好"  
			android:summary="通过意图启动一个Activity">  
		    <intent android:action="android.intent.action.VIEW"  
			    android:data="http://www.android.com" />  
		</PreferenceScreen>  
	    </PreferenceCategory>  
	    <PreferenceCategory  
		    android:title="偏好属性">  
		<CheckBoxPreference  
			android:key="parent_checkbox_preference"  
			android:title="父开关"  
			android:summary="这是一个父开关" />  
		<CheckBoxPreference  
			android:key="child_checkbox_preference"  
			android:dependency="parent_checkbox_preference"  
			android:layout="?android:attr/preferenceLayoutChild"  
			android:title="子开关"  
			android:summary="这是一个子开关" />  
	    </PreferenceCategory>  
	</PreferenceScreen>  

>

---

>

### 补充

>

	    <SwitchPreference
		android:summaryOff="已关闭"
		android:summaryOn="已开启"
		android:title="无线状态(WIFI)" />


