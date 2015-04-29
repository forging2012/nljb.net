---
title: Android之替代ListView的RecyclerView
date: '2015-04-29'
description:
categories:

tags:android

---

>

### RecyclerView

>

RecyclerView是android-support-v7-21版本中新增的一个Widgets

官方介绍RecyclerView是ListView的升级版本，更加先进和灵活。

我们写一个简单的实例例，来看一下究竟有多先进和灵活。

>

	// 支持库
	com.android.support:recyclerview-v7:22.0.0

>

<img src="{{urls.media}}/Android之替代ListView的RecyclerView/1.png" alt="" width="400" height="600">
<img src="{{urls.media}}/Android之替代ListView的RecyclerView/2.png" alt="" width="400" height="600">

>

RecyclerView 首先的一个特点就是，将 layout 抽象成了一个 LayoutManager

RecylerView 不负责子 View 的布局，我们可以自定义 LayoutManager 来实现不同的布局效果

目前只提供了LinearLayoutManager可以指定方向，默认是垂直，可指定水平，实现了水平的ListView。

>

---

>

RecyclerView 的另一个特点是标准化了 ViewHolde

编写 Adapter 面向的是 ViewHoder 而不在是View 了， 复用的逻辑被封装了， 写起来更加简单。

>

---

>

RecyclerView作为替代ListView使用，RecyclerView标准化了ViewHolder，ListView中convertView是复用的

在RecyclerView中，是把ViewHolder作为缓存的单位了，然后convertView作为ViewHolder的成员变量保持在ViewHolder中

也就是说，假设没有屏幕显示10个条目，则会创建10个ViewHolder缓存起来，每次复用的是ViewHolder

所以他把getView这个方法变为了onCreateViewHolder。 ViewHolder更适合多种子布局的列表，尤其IM的对话列表。

RecyclerView不提供setOnItemClickListener方法，你可以在ViewHolder中添加事件。RecyclerView的使用可以参考

>

---

	// 一个定义了RecyclerView的(Layout)XML
	// recycler.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:tools="http://schemas.android.com/tools"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	tools:context=".MyActivity">
	
	// RecyclerView	
	<android.support.v7.widget.RecyclerView
		android:id="@+id/recyclerView"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:scrollbars="vertical" />
	
	</RelativeLayout>

---

	// recycler_adapter_item(Layout-XML)
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:orientation="horizontal"
	    android:padding="10dp">

	    <LinearLayout
		android:id="@+id/linearLayout"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:orientation="vertical">

		<TextView
		    android:id="@+id/item_name"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:text="Name"
		    android:textSize="18sp" />

		<TextView
		    android:id="@+id/item_description"
		    android:layout_width="match_parent"
		    android:layout_height="wrap_content"
		    android:layout_marginTop="2dp"
		    android:text="Description"
		    android:textColor="#888"
		    android:textSize="12sp" />

	    </LinearLayout>

	</RelativeLayout>

---

	// 在Activity中使用RecyclerView
	public class MyRecyclerView extends Activity {

	    private RecyclerView recycler;

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		// 定义方法一
		setContentView(R.layout.recycler);
		recycler = (RecyclerView) findViewById(R.id.recyclerView);

		// 定义方法二
		recycler = new RecyclerView(this);
		setContentView(recycler);

		// 通过LinearLayoutManager设置方向
		LinearLayoutManager manager = new new LinearLayoutManager(this);

		// 设置方向
		manager.setOrientation(LinearLayoutManager.HORIZONTAL);

		// 设置 LayoutManager
		recycler.setLayoutManager(manager);

		// 通过ViewHoder设置内容
		recycler.setAdapter(new RecyclerAdapter());

	    }
	}

---

	// RecyclerView.Adapter
	class RecyclerAdapter extends RecyclerView.Adapter {
	
	    // RecyclerView.ViewHolder
	    class ViewHolder extends RecyclerView.ViewHolder {

		private TextView item_name;
		private TextView item_description;

		public ViewHolder(View itemView) {
		    super(itemView);
		    item_name = (TextView) itemView.findViewById(R.id.item_name);
		    item_description = (TextView) itemView.findViewById(R.id.item_description);
		}

		public TextView getItem_description() {
		    return item_description;
		}

		public TextView getItem_name() {
		    return item_name;
		}

	    }

	    @Override
	    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
		// 这里在onCreate的ViewHolder时加载一个View
		return new ViewHolder(LayoutInflater.from(parent.getContext()).inflate(R.layout.recycler_adapter_item, null));
	    }

	    @Override
	    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
		// 这里通过操作载入的View来进行设置内容及事件等
		ViewHolder vh = (ViewHolder) holder;
		vh.getItem_name().setText("标题 +" + String.valueOf(position));
		vh.getItem_description().setText("说明 +" + String.valueOf(position));
	    }

	    @Override
	    public int getItemCount() {
		// 内容的数量
		return 10;
	    }
	}



