---
title: Android之PullToRefresh的使用方法
date: '2015-03-16'
description:
categories:

tags:android

---

>

### PullToRefresh 是一套实现非常好的下拉刷新库

>

	// 支持：
	1.ListView
	2.ExpandableListView
	3.GridView
	4.WebView
	...

>

---

>

### 补充 setMode ...

>

* BOTH:上拉刷新和下拉刷新都支持 
* DISABLED：禁用上拉下拉刷新
* PULL_FROM_START:仅支持下拉刷新（默认）
* PULL_FROM_END：仅支持上拉刷新 
* MANUAL_REFRESH_ONLY：只允许手动触发

>

---

>

### android studio 导入 PullToRefresh

>

	// 下载
	https://github.com/chrisbanes/Android-PullToRefresh

	// 解压，得到 extras，sample，library(主要)
	Android-PullToRefresh-master -> library

	// 在你项目的根目录创建一个lib目录
	[PATH]
	[app]
		[src]
		[res]
		[build]
		build.gradle
		...
	[build]
	[gradle]
	[lib]
		// 将 library 重名名为 pull
		[pull]
			[src]
			[res]
			[build]
			...	
	build.gradle
	settings.gradle
	...

	// 修改 settings.gradle 
	include ':app', ':lib:pull'

	// 修改 [app]/build.gradle
	dependencies {
	    // Library
	    compile project(':lib:pull')
	}

	// 创建 [lib]/[pull]/build.gradle
	apply plugin: 'android-library'
	android {
	    compileSdkVersion 17
	    buildToolsVersion "21.1.2"

	    sourceSets {
		main {
		    manifest.srcFile 'AndroidManifest.xml'
		    java.srcDirs = ['src']
		    resources.srcDirs = ['src']
		    aidl.srcDirs = ['aidl']
		    renderscript.srcDirs = ['src']
		    res.srcDirs = ['res']
		    assets.srcDirs = ['assets']
		}
	    }
	}

	// 完成	

>

---

>

### 使用 PullToRefresh 刷新 ListView 

>

	//  在 activity_main.xml 增加一个 PullToRefreshListView
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent" >

	    <com.handmark.pulltorefresh.library.PullToRefreshListView
		android:id="@+id/left_drawer"
		android:layout_width="240dp"
		android:layout_height="match_parent"
		android:layout_gravity="start"
		android:choiceMode="singleChoice"
		android:dividerHeight="1px"
		android:divider="#ff000000"
		android:background="#ffffffff"
		android:layout_alignParentTop="true"
		android:layout_alignParentEnd="true"
		android:layout_alignParentStart="true" />
	</RelativeLayout>

	// 在 MainActivity
	public class MainActivity extends Activity {

	    PullToRefreshListView lv;

	    private String[] mListTitle = {"姓名", "性别", "年龄", "居住地", "邮箱"};
	    private String[] mListStr = {"雨松MOMO", "男", "25", "北京",
		    "xuanyusong@gmail.com"};
	    ListView mListView = null;
	    ArrayList<Map<String, Object>> mData = new ArrayList<Map<String, Object>>();

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 获取 PullToRefreshListView View
		lv = (PullToRefreshListView) findViewById(R.id.left_drawer);

		// ....
		int lengh = mListTitle.length;
		for (int i = 0; i < lengh; i++) {
		    Map<String, Object> item = new HashMap<String, Object>();
		    item.put("title", mListTitle[i]);
		    item.put("text", mListStr[i]);
		    mData.add(item);
		}

		// ...
		SimpleAdapter adapter = new SimpleAdapter(this, mData, android.R.layout.simple_list_item_2,
			 new String[]{"title", "text"}, new int[]{android.R.id.text1, android.R.id.text2});

		// 将 SimpleAdapter 设置到 PullToRefreshListView
		lv.setAdapter(adapter);

		// 刷新事件监听
		lv.setOnRefreshListener(new PullToRefreshBase.OnRefreshListener<ListView>() {
		    @Override
		    public void onRefresh(PullToRefreshBase<ListView> refreshView) {

			// ...
			new AsyncTask<Void, Void, Void>() {

			    @Override
			    protected Void doInBackground(Void... params) {

				// 处理刷新任务
				try {
				    Thread.sleep(3000);
				} catch (InterruptedException e) {
				    e.printStackTrace();
				}
				return null;
			    }

			    @Override
			    protected void onPostExecute(Void reslst)
			    {
				// 更行内容，通知 PullToRefresh 刷新结束
				lv.onRefreshComplete();
			    }

			}.execute();

		    }
		});

	    }

	}

