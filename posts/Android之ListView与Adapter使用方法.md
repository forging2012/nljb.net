---
title: Android之ListView与Adapter使用方法
date: '2015-03-16'
description:
categories:

tags:android

---

>

### ListView

>

        // ViewGroup -> AdapterView -> ... -> ListView
        // Adapter -> SpinnerAdapter -> BaseAdapter ->  ArrayAdapter ....
        // ListView，GridView，等都是容器，而Adapter负责提供每个“列表项”组件
        // AdapterView则负责采用合适的方式显示这些“列表项”
        // setAdapter(Adapter)是设置Adater的函数方法.

        // 简单，易用，通常用于将数组或List集合的多个值包装成多个列表项
        ArrayAdapter
        // 并不简单，功能强大，可用于将List集合的多个对象包装成多个列表项
        SimpleAdapter
        // 与 SimpleAdapter 相似，只是用于包装 Cursor 提供的数据
        SimpleCursorAdapter
        // 通常用于被扩展，扩展可以对各列表项进行最大限度的定制.
        BaseAdapter

>

---

>

### 关于 ListView 中 onItemClick 的 position 非 0 开始

>

	// 主要遇到两种
	//	一，插入 HeaderViews 造成 position 非 0
	//	二，使用 PullToRefreshListView 造成 position 非 0

	// 解决办法
    mRefreshable.getRefreshableView().setOnItemClickListener(new AdapterView.OnItemClickListener() {
		@Override
		public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
			if (position - mRefreshable.getRefreshableView().getHeaderViewsCount() == -1) {
				...
			} else if (position - mRefreshable.getRefreshableView().getHeaderViewsCount() >= 0) {
				...
			}
	}

	// ...
	// 基数页
	int page = 1;
	// 基数
	int base = 7;
	// 更多
	if (more) {
		int viewCount = mListView.getRefreshableView().getCount();
		int headerCound = mListView.getRefreshableView().getHeaderViewsCount();
		int footerCound = mListView.getRefreshableView().getFooterViewsCount();
		int count = viewCount - headerCound - footerCound;
		if (count % base == 0) {
			page = count / base + 1;
		} else {
			return;
		}
	}

>

---

>

### 关于 ListView 支持长按事件

>

		// Long Click 监听
		service_listview.setOnItemLongClickListener(this);

		// Long Click 监听
		@Override
		public boolean onItemLongClick(AdapterView<?> parent, View view, int position, long id) {
			switch (position) {
				case 0:
					break;
				default:
					AlertDialog.Builder builder = new AlertDialog.Builder(this);
					builder.setMessage("您确定要取消该预约?");
					builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
						@Override
						public void onClick(DialogInterface dialog, int which) {
						}
					});
					builder.setNegativeButton("返回", new DialogInterface.OnClickListener() {
						@Override
						public void onClick(DialogInterface dialog, int which) {
						}
					});
					builder.create().show();
					break;
			}
			return false;
		}

---

>

### 关于 ListView 禁用回弹

>

不同的安卓厂商对ListView或ScrollView都做了一些动画效果，比如下拉时为了产生弹性美感而有大幅度回弹效果

>

再比如魅族的下拉悬停，有时做了一个下拉刷新的功能会与之冲突。其实该美化实为多此一举。

>

	// 实际上可以通过设置ListView的一个ScrollMode属性来禁用
	listView.setOverScrollMode(View.OVER_SCROLL_NEVER);

>

---

>

### 关于 ListView Item 焦点问题

>

	项目中的listview不仅仅是简单的文字，常常需要自己定义listview，自己的Adapter去继承BaseAdapter
	可能会发生点击每一个item的时候没有反应，无法获取的焦点。

	原因多半是由于在你自己定义的Item中存在诸如ImageButton，Button，CheckBox等子控件
	(也可以说是Button或者Checkable的子类控件)，此时这些子控件会将焦点获取到
	所以常常当点击item时变化的是子控件，item本身的点击没有响应。

>

	这时候就可以使用descendantFocusability来解决啦，API描述如下：

	android:descendantFocusability

>

	该属性是当一个为view获取焦点时，定义viewGroup和其子控件两者之间的关系。

	属性的值有三种：

			beforeDescendants：viewgroup会优先其子类控件而获取到焦点
			afterDescendants： viewgroup只有当其子类控件不需要获取焦点时才获取焦点
			blocksDescendants：viewgroup会覆盖子类控件而直接获得焦点

>

---

>

### 关于 ListView 头部

>

	// ListView 可以通过 setHeaderView 来设置一个头部View
	View headerView = lif.inflate(R.layout.header, null);
	mListView.addHeaderView(headerView);

	...

>

---

>

### 关于 ListView 节头部

>

	// ListView 可以通过 SimplerExpandableListAdapter 
	// ListView 可以通过 ExpandableListView 
	// 可以处理分节列表中的二维数据结构

	...

>

---

>

### 禁用点击

>

	@Override
	public boolean isEnabled(int position) {
		return false;
	}

>

---

>

### 抢占焦点

>

关于在ListView的Item里增加(例CheckBox对象)造成Item无法点击问题

>

此问题是因为CheckBox对象抢占了Item的焦点，只需要在将CheckBox对象

>

	android:focusable属性值设置为false即可

>

---

>

### BaseAdapter

>

	// BaseAdapter 类
	class MenuBaseAdapter extends BaseAdapter {

	    // 上下文
	    private Context context;

	    // Activity
	    private MainActivity activity;

	    public MenuBaseAdapter(Context context) {
		this.context = context;
		this.activity = (MainActivity) context;
	    }

	    // 这里代表这ListView的行数量
	    // 也可以通过构造传入List或者其它办法
	    @Override
	    public int getCount() {
		// 项目数量
		return 7;
	    }

	    @Override
	    public Object getItem(int position) {
		return null;
	    }

	    @Override
	    public long getItemId(int position) {
		return 0;
	    }
		
	    // 在这里会按照getCount()的数量获取View
	    // 所以在这里定制View就可以了
	    @Override
	    public View getView(int position, View convertView, ViewGroup parent) {
		convertView = LayoutInflater.from(context).inflate(R.layout.menu_adapter_item, null);
		ImageView imageView = null;
		TextView textView = null;
		switch (position) {
		    case 0:
			imageView = (ImageView) convertView.findViewById(R.id.imageView);
			textView = (TextView) convertView.findViewById(R.id.textView);
			imageView.setImageResource(R.drawable.menu_lock_icon);
			convertView.setBackground(new ColorDrawable(0xff0997F7));
			textView.setText("登录系统");
			break;
		    case 1:
			imageView = (ImageView) convertView.findViewById(R.id.imageView);
			textView = (TextView) convertView.findViewById(R.id.textView);
			imageView.setImageResource(R.drawable.menu_bluetooth_icon);
			textView.setText("连接蓝牙");
			break;
		}
		// 返回
		return convertView;
	    }
	}

	// 创建一个自定义的BaseAdapter对象
	mMenuBaseAdapter = new MenuBaseAdapter(MainActivity.this);

	// 获取一个ListView对象
	ListView left_drawer = (ListView) findViewById(R.id.left_drawer);

	// 将BaseAdapter设置给ListView
	left_drawer.setAdapter(mMenuBaseAdapter);

	// 为ListView设置监听
	left_drawer.setOnItemClickListener(mMenuClickListener);

	// 监听
	private AdapterView.OnItemClickListener mMenuClickListener = new AdapterView.OnItemClickListener() {

		@Override
		public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
		    // position代表点击了ListView的第几个
		    switch (position) {
			case 0:
			    break;
		    }
		}
	};

>

---

>

### 补充Adapter适配器内存优化

>

在Android中Adapter使用十分广泛，特别是在list中。所以adapter是数据的 “集散地” 

>

所以对其进行内存优化是很有必要的。主要使用convertView和ViewHolder来进行缓存处理

>

	// ViewHolder
	class MenuViewHolder {
	    ImageView imageView;
	    TextView textView;
	}

	/**
	 * Created by nljb on 15/4/3.
	 */
	class MenuBaseAdapter extends BaseAdapter {

	    // 上下文
	    private Context context;

	    public MenuBaseAdapter(Context context) {
		this.context = context;
	    }

	    @Override
	    public int getCount() {
		// 项目数量
		return 7;
	    }

	    @Override
	    public Object getItem(int position) {
		return null;
	    }

	    @Override
	    public long getItemId(int position) {
		return 0;
	    }

	    @Override
	    public View getView(int position, View convertView, ViewGroup parent) {
		MenuViewHolder menuViewHolder = null;
		ImageView imageView = null;
		TextView textView = null;
		if (convertView == null) {
		    convertView = LayoutInflater.from(context).inflate(R.layout.menu_adapter_item, null);
		    imageView = (ImageView) convertView.findViewById(R.id.imageView);
		    textView = (TextView) convertView.findViewById(R.id.textView);
		    menuViewHolder = new MenuViewHolder();
		    menuViewHolder.imageView = imageView;
		    menuViewHolder.textView = textView;
		    convertView.setTag(menuViewHolder);
		} else {
		    menuViewHolder = (MenuViewHolder) convertView.getTag();
		    imageView = menuViewHolder.imageView;
		    textView = menuViewHolder.textView;
		}
		switch (position) {
		    case 0:
			imageView.setImageResource(R.drawable.menu_lock_icon);
			convertView.setBackground(new ColorDrawable(0xff0997F7));
			textView.setText("登录系统");
			break;
		    case 1:
			imageView.setImageResource(R.drawable.menu_bluetooth_icon);
			textView.setText("连接蓝牙");
			break;
		    case 2:
			imageView.setImageResource(R.drawable.menu_tasks_icon);
			textView.setText("命令列表");
			break;
		    case 3:
			imageView.setImageResource(R.drawable.menu_tasks_icon);
			textView.setText("配置列表");
			break;
		    case 4:
			imageView.setImageResource(R.drawable.menu_qrcode_icon);
			textView.setText("扫码配置");
			break;
		    case 5:
			imageView.setImageResource(R.drawable.menu_config_icon);
			textView.setText("系统设置");
			break;
		    case 6:
			imageView.setImageResource(R.drawable.menu_conn_icon);
			textView.setText("退出系统");
			break;
		}
		// 返回
		return convertView;
	    }

	}

