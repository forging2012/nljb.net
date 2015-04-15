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
