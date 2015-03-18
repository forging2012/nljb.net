---
title: Android之SharedPreferences使用方法
date: '2015-03-18'
description:
categories:

tags:android

---

>

### SharedPreferences 介绍

>

SharedPreferences 是一种轻型的数据存储方式

它的本质是基于XML文件存储Key-Value键值对数据, 通常用来存储一些简单的配置信息

其存储位置在/data/data/<包名>/shared_prefs目录下

>

	// 支持的类型
	boolean
	int
	float
	long
	String

>

SharedPreferences 对象本身只能获取数据而不支持存储和修改, 存储修改是通过Editor对象实现。

>

---

>

### SharedPreferences 使用

>

	// activity_main
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
	    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity"
	    android:orientation="vertical">

		<EditText
		    android:id="@+id/edit_key"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:inputType="none" />


		<EditText
		    android:id="@+id/edit_val"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:inputType="none" />

		<Button
		    android:id="@+id/button_write"
		    android:text="添加"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content" />

		<Button
		    android:id="@+id/button_read"
		    android:text="获取"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:layout_gravity="center_horizontal|bottom" />

	</LinearLayout>

	// Activity
	public class MainActivity extends Activity {

	    private EditText edit_key;
	    private EditText edit_val;
	    private Button but_read;
	    private Button but_write;

	    SharedPreferences preferences;
	    SharedPreferences.Editor editor;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 获取 Preferences
		preferences = getPreferences(Activity.MODE_PRIVATE);
		// 编辑 preferences
		editor = preferences.edit();
		edit_key = (EditText) findViewById(R.id.edit_key);
		edit_val = (EditText) findViewById(R.id.edit_val);
		but_read = (Button) findViewById(R.id.button_read);
		but_write = (Button) findViewById(R.id.button_write);
		but_write.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			if (!edit_key.getText().toString().isEmpty() && !edit_val.getText().toString().isEmpty())
			{
			    // 使用 editor.putString 增加数据
			    editor.putString(edit_key.getText().toString(), edit_val.getText().toString());
			    // Commit 保存
			    if (editor.commit()){
				Toast.makeText(MainActivity.this, "写入成功", Toast.LENGTH_SHORT).show();
			    }
			} else {
			    Toast.makeText(MainActivity.this, "值不能为空", Toast.LENGTH_SHORT).show();
			}
		    }
		});
		but_read.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			if (!edit_key.getText().toString().isEmpty()){
			    // 读取 preferences.getString 
			    String val = preferences.getString(edit_key.getText().toString(), "default");
			    edit_val.setText(val);
			} else {
			    Toast.makeText(MainActivity.this, "值不能为空", Toast.LENGTH_SHORT).show();
			}
		    }
		});
	    }
	}


