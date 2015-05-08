---
title: Android之Fragment使用方法
date: '2015-03-11'
description:
categories:

tags:android

---

>

### 备注

>

---

>

Layout 占位供 Fragment 使用介绍

>

	// 当需要使用 Fragment 替换当前 View 里面某一个区域的时候
	// 就需要使用一个 Layout 进行布局，并占位，然后通过 ID 进行
	// Fragment 的 add 或 replace 或 rm 等操作 ...
	<android.support.v4.widget.DrawerLayout
	android:id="@+id/drawer_layout"
	android:layout_width="match_parent"
	android:layout_height="match_parent">
	<!-- The main content view -->
	// 这个就是 Layout 占位
	<FrameLayout
	    android:id="@+id/main"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent" />
	<!-- The navigation drawer -->
	<ListView
	    android:id="@+id/left_drawer"
	    android:layout_width="200dp"
	    android:layout_height="match_parent"
	    android:layout_gravity="start"
	    android:background="#f5f5f5f5"
	    android:choiceMode="singleChoice"
	    android:divider="#ffe0e0e0"
	    android:dividerHeight="1dp" />
	</android.support.v4.widget.DrawerLayout>

>
	
	// 这里就是向占位的 Layout 内替换一个 Fragment (其实不是替换Layout而是替换其内部的Fragment)
	FragmentTransaction transaction = getFragmentManager().beginTransaction();
	transaction.replace(R.id.main,new Fragment());
	transaction.commit();	

>

---

> 

Fragment 不使用占位介绍

>

	// 一个完整的 LinearLayout 需要被 Fragment 替换的
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:id="@+id/main"
	    android:layout_width="fill_parent"
	    android:layout_height="fill_parent"
	    android:orientation="horizontal"
	    android:padding="10dp">
	</LinearLayout>

        // 直接用 Fragment 替换整个 Layout (其实不是替换Layout而是替换其内部的Fragment)
        FragmentTransaction transaction = getFragmentManager().beginTransaction();
        transaction.replace(R.id.main,new Fragment());
        transaction.commit();

>

---

>

### Fragment 介绍

>

	// 对话框界面
	DialogFragment
	// 实现列表界面
	ListFragment
	// 选项设置界面
	PreferenceFragment
	// WebView界面
	WebViewFragment

>

---

>

	// Fragment 可以被 add 或者 rm 或 replace 到 Layout 中 ...
	public class MainActivity extends Activity {
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
			MyFragment fragment = new MyFragment();
			FragmentTransaction ft = getFragmentManager().beginTransaction() ;
			ft.add(R.id.framelayout, fragment);
			ft.commit() ;
		}
	}

>

---

>

	// 主要有三点
	1，有个主(FrameLayout)供你添加或替换(Fragment)
	2，有几个(Fragment)供你添加到(FrameLayout)
	3，有个事件触发(Fragment)间的添加或者替换(Fragment)

	// Fragment生命周期的核心函数
	// 从fragment运行起来到恢复状态(跟用户紧密相关的)有：
	onAttach(Activity) 
	// 当Fragment关联activity时调用onAttach（Activity）。
	onCreate(Bundle) 
	// 第一次创建Fragment的时候调用onCreate（Bundle）。
	onCreateView(LayoutInflater, ViewGroup, Bundle) 
	// 创建一个跟Fragment关联的视图层级。
	onActivityCreated(Bundle) 
	// 函数告诉Fragment跟它关联的activity已经完成Activity.onCreaate。
	onStart() 
	// 函数可以让用户看见Fragment（条件是Activity已经启动）。
	onResume() 
	// 使Fragment跟用户交互（条件是Activity已经恢复了）
	// 当一个Fragment不再使用，它必须经历一系列反向回调：
	onPause() 
	// 不论因为Activity被暂停或者Fragment的操作正在修改导致Fragment不再和用户交互。
	onStop() 
	// 不论因为Activity已经停止或者Fragment的操作正在修改导致用户不能看见Fragment。
	onDestroyView() 
	// Fragment清理和它关联的View的资源。
	onDestroy()
	// 清理Fragment的状态。
	onDetach()
	// 在Fragment不再和Activity关联之前调用onDetach()。	

>

	// 一，主(FrameLayout)
	// activity_main.xml
	<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:id="@+id/container"
	    android:layout_width="match_parent" android:layout_height="match_parent"
	    tools:context=".MainActivity" tools:ignore="MergeRootFrame" >

	    // 这个主(FrameLayout)已经有个内置的(Fragment)了
	    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
		android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
		android:paddingRight="@dimen/activity_horizontal_margin"
		android:paddingTop="@dimen/activity_vertical_margin"
		android:paddingBottom="@dimen/activity_vertical_margin"
		tools:context=".MainActivity$PlaceholderFragment">

		<Button
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:text="New Button"
		    android:id="@+id/button"
		    android:layout_below="@+id/textView"
		    android:layout_alignParentLeft="true"
		    android:layout_marginTop="173dp" />

	    </RelativeLayout>

	</FrameLayout>

	// 二，主类
	// MainActivity.class
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 这里将一个(Fragment)添加到(FrameLayout)中
		if (savedInstanceState == null) {
		    getFragmentManager().beginTransaction()
			    // 添加
			    .add(R.id.container, new PlaceholderFragment())
			    .commit();
		}
		Button button = (Button) findViewById(R.id.button);
		button.setOnClickListener(new View.OnClickListener() {
		    @Override
		    public void onClick(View v) {
			// Bundle 存值
			Bundle bundle = new Bundle();
			bundle.putString("key", "value");
			// Fragment 对象
			Fragment fr = new Fragment1();
			// 保存
			fr.setArguments(bundle);
			// 这里将一个(Fragment)替换到(FrameLayout)中的(Fragment)
			getFragmentManager().beginTransaction()
				// 替换
				.replace(R.id.container, fr)
				.commit();
		    }
		});
	    }

	    // 创建了一个(Fragment)并返回(Fragment)的(xLayout)
	    public static class PlaceholderFragment extends Fragment {

		public PlaceholderFragment() {
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
					 Bundle savedInstanceState) {
		    View rootView = inflater.inflate(R.layout.fragment_main, container, false);
		    return rootView;
		}
	    }

	   // 创建了一个(Fragment)并返回(Fragment)的(Layout)
	   public static class Fragment1 extends Fragment {

		public Fragment1() {
		}

		String val;
		Button button;

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
					 Bundle savedInstanceState) {
		    // 获取传过来的值
		    val  = getArguments().getString("key");
		    Log.i("info", val);
		    // 初始化 View
		    View rootView = inflater.inflate(R.layout.fragment, container, false);
		    // 获取 Button
		    button = (Button) rootView.findViewById(R.id.button2);
		    // 监听 Button 事件
		    button.setOnClickListener(new View.OnClickListener() {
			@Override
			public void onClick(View v) {
			    // 将值设置为按钮名
			    button.setText(val);
			}
		    });

		    return rootView;
		}
	    }

	// 三，两个(Layout)
	// fragment.xml
	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent" android:layout_height="match_parent">

	    <Switch
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="New Switch"
		android:id="@+id/switch1" />

	    // 加个按钮，上面演示了如何使用该按钮
	    <Button
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:text="New Button"
		android:id="@+id/button2" />

	</LinearLayout>

	// fragment_main.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
	    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    tools:context=".MainActivity$PlaceholderFragment">

	    <TextView android:text="@string/hello_world" android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:id="@+id/textView" />

	</RelativeLayout>

---

>

### ListFragment 介绍

>

### FragmentManager 介绍

>

### FragmentTransaction 介绍

>

	FragmentManager能够实现管理activity中fragment. 通过调用activity的getFragmentManager()取得它的实例.
	FragmentManager可以做如下一些事情:
		1、使用findFragmentById() (用于在activity layout中提供一个UI的fragment)或findFragmentByTag()
		   (适用于有或没有UI的fragment)获取activity中存在的fragment
		2、将fragment从后台堆栈中弹出, 使用 popBackStack() (模拟用户按下BACK 命令).
		3、使用addOnBackStackChangeListener()注册一个监听后台堆栈变化的listener.

>

	// FragmentTransaction：
	// FragmentTransaction对fragment进行添加,移除,替换,以及执行其他动作。
	// 从 FragmentManager 获得一个FragmentTransaction的实例 :
	FragmentManager fragmentManager = getFragmentManager(); 
	FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

	// 每一个事务都是同时要执行的一套变化.可以在一个给定的事务中设置你想执行的所有变化
	// 使用诸如 add(), remove(), 和 replace().然后, 要给activity应用事务, 必须调用 commit().
	// 在调用commit()之前, 你可能想调用 addToBackStack(),将事务添加到一个fragment事务的back stack.
	//  这个back stack由activity管理, 并允许用户通过按下 BACK 按键返回到前一个fragment状态.

	// Create new fragment and transaction  
	Fragment newFragment = new ExampleFragment();  
	FragmentTransaction transaction = getFragmentManager().beginTransaction();  
	// Replace whatever is in the fragment_container view with this fragment,  
	// and add the transaction to the back stack  
	transaction.replace(R.id.fragment_container, newFragment);  
	transaction.addToBackStack(null);  
	// Commit the transaction  
	transaction.commit();

>

FragmentManager（碎片管理器），用来管理当前Activity中所有的Fragment

>

每次替换或者添加后，都要commit一样，才能算一个完整的事务，这里用了Fragment嵌套

如果你是嵌套了Fragment，那么使用FragmentManager的一定要注意你当前的Fragment是属于嵌套的还是顶层的Fragment

如果是顶层Fragment，那么你调用FragmentManager的时候，应该这样写getActivity().getSupportFragmentManager()

如果是嵌套的fragment那么应该这样写getChildFragmentManager()

>

	// activity_main.xml
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="horizontal"
	    tools:context=".MainActivity" >

	    <LinearLayout
		android:id="@+id/left"
		android:layout_width="0dp"
		android:layout_height="match_parent"
		android:layout_weight="1"
		android:background="#CCCCCC"
		android:orientation="vertical" >

		<Button
		    android:id="@+id/button"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:text="显示列表" />
	    </LinearLayout>

	    <LinearLayout
		android:id="@+id/center"
		android:layout_width="0dp"
		android:layout_height="match_parent"
		android:layout_weight="2"
		android:background="#CCDDFF"
		android:orientation="vertical" >
	    </LinearLayout>

	</LinearLayout>


	// Activity
	public class MainActivity extends Activity {

	    private Button button;
	    private FragmentManager manager;
	    private FragmentTransaction transaction;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		manager = getFragmentManager();

		button = (Button) findViewById(R.id.button);
		/**
		 * 点击Activity中的该按钮，Activity会在布局中间添加ArticleListFragment，并显示列表数据。
		 */
		button.setOnClickListener(new View.OnClickListener() {

		    @Override
		    public void onClick(View v) {
			transaction = manager.beginTransaction();
			ArticleListFragment articleListFragment = new ArticleListFragment();
			Bundle a = new Bundle();
			a.putString("key", "center");
			articleListFragment.setArguments(a);
			transaction.add(R.id.center, articleListFragment, "center");
			transaction.commit();
		    }
		});
	    }
	}

	// ListFragment
	public class ArticleListFragment extends ListFragment {

	    private ArrayAdapter<String> adapter;
	    private List<String> data;
	    private FragmentManager manager;
	    private FragmentTransaction transaction;

	    @Override
	    public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		String k = (String) getArguments().get("key");
		data = new ArrayList<String>();
		for (int i = 0; i < 30; i++) {
		    data.add(k + i);
		}
		manager = getFragmentManager();
		adapter = new ArrayAdapter<String>(getActivity(),
			android.R.layout.simple_list_item_1, data);
		// 设置一个 Adapter
		setListAdapter(adapter);
	    }

	    @Override
	    public void onListItemClick(ListView l, View v, int position, long id) {
		super.onListItemClick(l, v, position, id);
		// 获取单机的 Item
		String str = adapter.getItem(position);
		// 从 FragmentManager 获得一个FragmentTransaction的实例 :
		// FragmentManager fragmentManager = getFragmentManager();
		// FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
		// transaction.add(); transaction.replace(); transaction.remove();
		transaction = manager.beginTransaction();
	    }
	}

