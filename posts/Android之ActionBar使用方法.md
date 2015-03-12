---
title: Android之ActionBar使用方法
date: '2015-03-12'
description:
categories:

tags:android

---

>

### ActionBar 

>
	
	// 同时隐藏图标与标题，TABS将覆盖标题栏.
	public class MainActivity extends Activity {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	    }

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.menu_main, menu);
		// 获取 ActionBar
		final ActionBar actionBar = getActionBar();
		// 隐藏 ActionBar 标题
		actionBar.setDisplayShowTitleEnabled(false);
		// 隐藏 ActionBar 图标
		actionBar.setDisplayShowHomeEnabled(false);
		// 将应用程序图标设置为可点击
		actionBar.setHomeButtonEnabled(true);
		// 将应用程序图标设置为可点击，并在图标上添加向左箭头
		actionBar.setDisplayHomeAsUpEnabled(true);
		// MenuItem.SHOW_AS_ACTION_ALWAYS 总是将该MenuItem显示在ActionBar上
		// MenuItem.SHOW_AS_ACTION_COLLAPSE_ACTION_VIEW 将该Action View折叠成普通菜单项
		// MenuItem.SHOW_AS_ACTION_IF_ROOM 当ActionBar位置足够时才显示在ActionBar上
		// MenuItem.SHOW_AS_ACTION_NEVER 不将该MenuItem显示在ActionBar上
		// MenuItem.SHOW_AS_ACTION_WITH_TEXT 将该MenuItem显示在ActionBar上，并显示该菜单的文本
		menu.add(0, 0x101, 0, "普通菜单").setIcon(R.mipmap.ic_launcher).setShowAsAction(MenuItem.SHOW_AS_ACTION_IF_ROOM);
		menu.add(0, 0x102, 0, "普通菜单").setIcon(R.mipmap.ic_launcher).setShowAsAction(MenuItem.SHOW_AS_ACTION_IF_ROOM);
		// 返回
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

		switch (id)
		{
		    // 判断是否为主按钮点击
		    case android.R.id.home:
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
			break;
		    case 0x101:
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
			break;
		}

		return super.onOptionsItemSelected(item);
	    }


---

>

### ActionBar 之 ActionBar.TabListener

>

	public class MainActivity extends Activity implements  ActionBar.TabListener {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	    }

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		//getMenuInflater().inflate(R.menu.menu_main, menu);
		// 获取 ActionBar
		final ActionBar actionBar = getActionBar();
		// 隐藏 ActionBar 标题
		actionBar.setDisplayShowTitleEnabled(false);
		// 隐藏 ActionBar 图标
		// actionBar.setDisplayShowHomeEnabled(false);
		// ActionBar TABS 模式
		actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_TABS);
		actionBar.addTab(actionBar.newTab().setText("第一页").setTabListener((ActionBar.TabListener) MainActivity.this));
		actionBar.addTab(actionBar.newTab().setText("第二页").setTabListener((ActionBar.TabListener) MainActivity.this));
		actionBar.addTab(actionBar.newTab().setText("第三页").setTabListener((ActionBar.TabListener) MainActivity.this));
		// 返回
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

		return super.onOptionsItemSelected(item);
	    }

	    @Override
	    public void onTabSelected(ActionBar.Tab tab, FragmentTransaction ft) {
		// 自定义的 Fragment
		Fragment1 fragment;
		// 获取 TAB 编号，0, 1, 2, 3, .....
		if (tab.getPosition() == 1) {
		    fragment = new Fragment1();
		    // 传参
		    Bundle args = new Bundle();
		    args.putString("key", "value");
		    fragment.setArguments(args);
		    FragmentTransaction f = getFragmentManager().beginTransaction();
		    // 替换，也可以 ADD 
		    f.replace(R.id.xxx, fragment);
		    f.commit();
		}
	    }

	    @Override
	    public void onTabUnselected(ActionBar.Tab tab, FragmentTransaction ft) {

	    }

	    @Override
	    public void onTabReselected(ActionBar.Tab tab, FragmentTransaction ft) {

	    }

	    // 创建了一个(Fragment)并返回(Fragment)的(Layout)
	    public static class Fragment1 extends Fragment {

		public Fragment1() {
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
					 Bundle savedInstanceState) {
		    // 初始化 View
		    View rootView = inflater.inflate(R.layout.fragment_blank, container, false);
		    TextView text = (TextView) rootView.findViewById(R.id.text);
		    // 获取传值
		    Bundle args = getArguments();
		    text.setText(args.getString("key"));
		    return rootView;
		}
	    }

	}


---

>

### ActionBar 之 ActionBar.OnNavigationListener

>

	public class MainActivity extends Activity implements  ActionBar.OnNavigationListener {

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	    }

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		//getMenuInflater().inflate(R.menu.menu_main, menu);
		// 获取 ActionBar
		final ActionBar actionBar = getActionBar();
		// 隐藏 ActionBar 标题
		actionBar.setDisplayShowTitleEnabled(false);
		// 隐藏 ActionBar 图标
		actionBar.setDisplayShowHomeEnabled(false);
		// ActionBar LIST 模式
		actionBar.setNavigationMode(ActionBar.NAVIGATION_MODE_LIST);
		actionBar.setListNavigationCallbacks(
			new ArrayAdapter<String>(MainActivity.this,
				android.R.layout.simple_list_item_1,
				android.R.id.text1, new String[]
				{"第一页", "第二页", "第三页"}), this);
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

		return super.onOptionsItemSelected(item);
	    }

	    @Override
	    public boolean onNavigationItemSelected(int itemPosition, long itemId) {
		// 获取选择
		return false;
	    }

	}


---

>

### ActionBar 自定义 

>

	public class MainActivity extends Activity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// 自定义actionbar的布局
		setActionBarLayout(R.layout.menu);
	    }

	    //
	    public void setActionBarLayout(int layoutId) {
		// 获取 ActionBar
		ActionBar actionBar = getActionBar();
		if (null != actionBar) {
		    // 隐藏 ActionBar 图标
		    actionBar.setDisplayShowHomeEnabled(false);
		    // 使自定义的普通View能在title栏显示
		    actionBar.setDisplayShowCustomEnabled(true);
		    // 载入 Layout
		    View v = this.getLayoutInflater().inflate(layoutId, null);
		    // 设置 Layout
		    ActionBar.LayoutParams layout = new ActionBar.LayoutParams(ActionBar.LayoutParams.MATCH_PARENT, ActionBar.LayoutParams.MATCH_PARENT);
		    // 将 Layout 赋值 ActionBar
		    actionBar.setCustomView(v, layout);
		}
	    }

	    public void onClick(View v) {
		switch (v.getId()) {
		    case R.id.menuBtnId: {
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
		    }
		    break;
		    case R.id.noteBtnId: {
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
		    }
		    break;
		    case R.id.downloadBtnId: {
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
		    }
		    break;
		    case R.id.editBtnId: {
			Toast.makeText(getApplicationContext(), "欢迎", Toast.LENGTH_SHORT).show();
		    }
		    break;
		    default: {
		    }
		    break;
		}
	    }

	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		// getMenuInflater().inflate(R.menu.menu_main, menu);
		// 返回
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
		return super.onOptionsItemSelected(item);
	    }

	}
