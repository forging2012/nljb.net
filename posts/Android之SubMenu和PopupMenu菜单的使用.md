---
title: Android之SubMenu和PopupMenu菜单的使用
date: '2015-03-10'
description:
categories:

tags:android

---
>

### 选项菜单 SubMenu

>

	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
	    }

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
		// 普通菜单
		menu.add(0, 0x101, 0, "普通菜单");
		// 包含子菜单
		SubMenu fontMenu = menu.addSubMenu("字体大小");
		fontMenu.setHeaderTitle("请选择字体大小");
		fontMenu.setIcon(R.mipmap.ic_launcher);
		fontMenu.setHeaderIcon(R.mipmap.ic_launcher);
		fontMenu.add(0, 0x111, 0, "10 号字体");
		fontMenu.add(0, 0x112, 0, "11 号字体");
		// 为菜单项目添加关联的 Activity
		MenuItem item = fontMenu.add(0, 0x113, 0, "12 号字体");
		item.setIntent(new Intent(MainActivity.this, GestureActivity.class));
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.menu_main, menu);
		return true;
	    }

	    @Override
	    public boolean onOptionsItemSelected(MenuItem item) {
		// Handle action bar item clicks here. The action bar will
		// automatically handle clicks on the Home/Up button, so long
		// as you specify a parent activity in AndroidManifest.xml.
		int id = item.getItemId();

		//noinspection SimplifiableIfStatement
		if (id == R.id.action_settings) {
		    return true;
		}

		// 判断单机的是那一个菜单
		switch (id)
		{
		    case 0x101:
			Toast.makeText(MainActivity.this, "您选择了(普通菜单)", Toast.LENGTH_SHORT).show();
			// 也可以判断单机的是哪一个菜单，来 Intent 到另一个 Activity
			Intent intent = new Intent(MainActivity.this, GestureActivity.class);
			startActivity(intent);
			break;
		    case 0x111:
			Toast.makeText(MainActivity.this, "您选择了(10 号字体)", Toast.LENGTH_SHORT).show();
			break;
		    case 0x112:
			Toast.makeText(MainActivity.this, "您选择了(11 号字体)", Toast.LENGTH_SHORT).show();
			break;
		    case 0x113:
			Toast.makeText(MainActivity.this, "您选择了(12 号字体)", Toast.LENGTH_SHORT).show();
			break;
		}

		return super.onOptionsItemSelected(item);
	    }

	}

---

>

### PopupMenu 创建弹出式菜单 

>

	// 加个按钮，用于触发 show(View v)
	<Button
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:text="Go"
	android:id="@+id/button9"
	android:onClick="show"
	android:layout_alignParentTop="true"
	android:layout_centerHorizontal="true" />

	// R.menu.menu_control
	<menu xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context="com.example.nljb.nljb.ControlActivity">
	    <item
		android:id="@+id/open"
		android:orderInCategory="100"
		android:showAsAction="never"
		android:title="打开"/>

	    <item
		android:id="@+id/close"
		android:orderInCategory="100"
		android:showAsAction="never"
		android:title="关闭"/>
	</menu>

	// Activity
	public class MainActivity extends Activity {

	    View root;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
	    }
	
	    // PopupMenu 函数
	    public void show(View v)
	    {
		PopupMenu pop = new PopupMenu(MainActivity.this, v);
		// 加载一个 R.menu.menu_control 
		pop.getMenuInflater().inflate(R.menu.menu_control, pop.getMenu());
		pop.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
		    public boolean onMenuItemClick(MenuItem item) {
			switch (item.getItemId())
			{
			    // 通过按钮ID来判断点击
			    case R.id.open:
				Toast.makeText(MainActivity.this, "Clicked popup menu item open" + item.getTitle(),
					Toast.LENGTH_SHORT).show();
				break;
			    // 通过按钮ID来判断点击
			    case R.id.close:
				Toast.makeText(MainActivity.this, "Clicked popup menu item close" + item.getTitle(),
					Toast.LENGTH_SHORT).show();
				break;
			}

			return true;
		    }
		});
		pop.show();
	    }
	}



