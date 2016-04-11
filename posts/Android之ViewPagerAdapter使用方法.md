---
title: Android之ViewPagerAdapter使用方法
date: '2015-03-24'
description:
categories:

tags:android

---

>

### PagerAdapter 介绍

>

	属于：android.support.v4.view.PagerAdapter;
	扩展：com.android.support:support-v4

>

	// activity_main.xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <android.support.v4.view.ViewPager
		android:id="@+id/viewpager"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="#00000000">
		</android.support.v4.view.ViewPager>

	</RelativeLayout>

	// one.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <ImageView
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="@drawable/guide_1"
		/>

	</LinearLayout>

	// two.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <ImageView
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="@drawable/guide_2"
		/>

	</LinearLayout>

	// three.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <ImageView
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="@drawable/guide_3"
		/>

	</LinearLayout>

	// Activity
	public class MainActivity extends Activity {

	    private ViewPager vp;
	    private ViewPagerAdapter vpa;
	    private List<View> views;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 获取 LayoutInflater
		LayoutInflater inflater = LayoutInflater.from(this);

		// 获取 View 并加入 List
		views = new ArrayList<View>();
		views.add(inflater.inflate(R.layout.one, null));
		views.add(inflater.inflate(R.layout.two, null));
		views.add(inflater.inflate(R.layout.three, null));

		// 将 ViewList 传入 ViewPagerAdapter
		vpa = new ViewPagerAdapter(views, this);

		// 获取 android.support.v4.view.ViewPager
		vp = (ViewPager) findViewById(R.id.viewpager);

		// 设置 Adapter
		vp.setAdapter(vpa);
	    }

	}

	//  ViewPagerAdapter
	public class ViewPagerAdapter extends PagerAdapter {

	    /*
		理解：PagerAdapter
		一，调用 getCount() 获取需要初始化的 ViewGroup 数量
		二，调用 instantiateItem() 实例化页卡，按顺序
		三，调用 destroyItem() 销毁，按顺序
	     */

	    // 所有 View
	    private List<View> views;

	    // 上下文
	    private Context context;

	    // 构造
	    public ViewPagerAdapter(List<View> views, Context context) {
		this.views = views;
		this.context = context;
	    }

	    // 销毁时被调用
	    @Override
	    public void destroyItem(ViewGroup container, int position, Object object) {
		// ViewGroup 所有的View
		// position 位置,第几个
		// 销毁时删除 View
		container.removeView(views.get(position));
		// super.destroyItem(container, position, object);
	    }

	    // 实例化页卡
	    @Override
	    public Object instantiateItem(ViewGroup container, int position) {
		// 添加 View
		container.addView(views.get(position));
		// 返回添加的 View 对象
		return views.get(position);
		// return super.instantiateItem(container, position);
	    }

	    // 所包含的 Item 总个数
	    @Override
	    public int getCount() {
		// 返回 views 总数
		return views.size();
	    }

	    @Override
	    public boolean isViewFromObject(View view, Object o) {
		return (view == o);
	    }

	}


---

>

### 设置启动页

>

	// 正常情况下 AndroidManifest.xml
	    <application
		android:allowBackup="true"
		android:icon="@mipmap/ic_launcher"
		android:label="@string/app_name"
		android:theme="@style/AppTheme" >
		<activity
		    android:name=".MainActivity"
		    android:label="@string/app_name" >
		    <intent-filter>
			<action android:name="android.intent.action.MAIN" />

			<category android:name="android.intent.category.LAUNCHER" />
		    </intent-filter>
		</activity>
		<activity
		    android:name=".MainActivity2Activity"
		    android:label="@string/title_activity_main_activity2" >
		</activity>
	    </application>

	// 作为启动页 AndroidManifest.xml
	    <application
		android:allowBackup="true"
		android:icon="@mipmap/ic_launcher"
		android:label="@string/app_name"
		android:theme="@style/AppTheme" >
		<activity
		    android:name=".MainActivity"
		    android:label="@string/app_name" >
		</activity>
		<activity android:name=".MainActivity2Activity">
		    <intent-filter>
			<action android:name="android.intent.action.MAIN" />

			<category android:name="android.intent.category.LAUNCHER" />
		    </intent-filter>
		</activity>
	    </application>


---


>

### 添加导航点

>

	// activity_main.xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <android.support.v4.view.ViewPager
		android:id="@+id/viewpager"
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="#00000000">
		</android.support.v4.view.ViewPager>

	    // 添加一排导航点 ImageView
	    <LinearLayout
		android:layout_width="fill_parent"
		android:layout_height="wrap_content"
		android:layout_alignParentBottom="true"
		android:gravity="center_horizontal"
		android:orientation="horizontal">

		<ImageView
		    android:id="@+id/iv1"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:src="@drawable/login_point_selected"/>

		<ImageView
		    android:id="@+id/iv2"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:src="@drawable/login_point"/>

		<ImageView
		    android:id="@+id/iv3"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:src="@drawable/login_point"/>
		
		</LinearLayout>

	</RelativeLayout>

>

	// Activity
	public class MainActivity extends Activity implements ViewPager.OnPageChangeListener {

	    private ViewPager vp;
	    private ViewPagerAdapter vpa;
	    private List<View> views;
	    private ImageView[] dots;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		// 获取 LayoutInflater
		LayoutInflater inflater = LayoutInflater.from(this);

		// 获取 View 并加入 List
		views = new ArrayList<View>();
		views.add(inflater.inflate(R.layout.one, null));
		views.add(inflater.inflate(R.layout.two, null));
		views.add(inflater.inflate(R.layout.three, null));

		// 将 ViewList 传入 ViewPagerAdapter
		vpa = new ViewPagerAdapter(views, this);

		// 获取 android.support.v4.view.ViewPager
		vp = (ViewPager) findViewById(R.id.viewpager);

		// 设置 Adapter
		vp.setAdapter(vpa);

		// 回调
		// vp.setOnPageChangeListener(new ViewPager.OnPageChangeListener() { ... };
		// 新版本改变: vPager.addOnPageChangeListener(this);
		vp.setOnPageChangeListener(this);

		// 添加 ImageView 对象到 ImageView[]
		dots = new ImageView[views.size()];
		dots[0] = (ImageView) findViewById(R.id.iv1);
		dots[1] = (ImageView) findViewById(R.id.iv2);
		dots[2] = (ImageView) findViewById(R.id.iv3);

	    }

	    // 当页面被滑动时
	    @Override
	    public void onPageScrolled(int i, float v, int i2) {

	    }

	    // 当前新的页面被选中时调用
	    @Override
	    public void onPageSelected(int i) {
		// 获取当前选择页
		// 修改 Image 图片
		for (int n = 0; n < dots.length; n++) {
		    if (i == n) {
			// R.drawable.login_point_selected 黑色
			dots[n].setImageResource(R.drawable.login_point_selected);
		    } else {
			// R.drawable.login_point 白色
			dots[n].setImageResource(R.drawable.login_point);
		    }
		}
	    }

	    // 当滑动页面改变时
	    @Override
	    public void onPageScrollStateChanged(int i) {

	    }

	}


---

>

### 添加进入按钮

>

	// three.xml
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <ImageView
		android:layout_width="fill_parent"
		android:layout_height="fill_parent"
		android:background="@drawable/guide_3"
		/>

	    <LinearLayout
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:layout_alignParentBottom="true"
		android:gravity="center_horizontal"
		android:orientation="horizontal">

		// 添加一个按钮
		<Button
		    android:id="@+id/start_but"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent"
		    android:text="进入" />

		</LinearLayout>

	</RelativeLayout>

	// Activity
	public class MainActivity extends Activity implements ViewPager.OnPageChangeListener {

	    private ViewPager vp;
	    private ViewPagerAdapter vpa;
	    private List<View> views;
	    private ImageView[] dots;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.gulde);

		......

		// 添加 ImageView 对象到 ImageView[]
		dots = new ImageView[views.size()];
		dots[0] = (ImageView) findViewById(R.id.iv1);
		dots[1] = (ImageView) findViewById(R.id.iv2);
		dots[2] = (ImageView) findViewById(R.id.iv3);

		// 这里注意，按钮是属于 R.id.iv3 的
		Button button = (Button) views.get(2).findViewById(R.id.start_but);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			Intent intent = new Intent(MainActivity.this, MainActivity2.class);
			startActivity(intent);
			finish();
		    }
		});

	    }
	}


