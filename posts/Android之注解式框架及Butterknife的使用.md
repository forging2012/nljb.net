---
title: Android之注解式框架及Butterknife的使用
date: '2015-06-24'
description:
categories:

tags:android

---

>

### 注解式框架

>

### 什么是注解式开发:

	JDK1.5后支持注解方式,想用注解式开发,就要自定义注解

	结构:
		@+注解名(也可以叫类名)+传递的属性值,key和value,
		可设置目标范围:方法(Method)、属性(Filed)、类(Type)

	自定义注解要用到
		@interface:用于定义注解;
		@Target:用于描述注解的使用范围;
		@Retention: 注解的生命周期,一般RetentionPolicy.RUNTIME

	在Android中使用一般是简化代码,提升开发效率,清晰简介

>

---

>

### 流的注解式框架有Dagger、ButerKnife、AndroidAnnotations。 

	AndroidAnnotations是一个利用注解方式来简化代码结构,提高开发效率的开源框架。
		一，配置麻烦,需要在项目清单里注册生成的子类。
		二，反射机制会占用资源内存和耗时。

	ButerKnife用起来方便,配置简单,强大的View注入绑定和简单的常用方法注解。

	Dagger采用预编译技术,高效,但是对View绑定操作注解不是很方便。

>

---

>

### ButerKnife特点: 

	强大方便的处理View绑定和Click事件,简化代码,提升开发效率

	方便的处理ListView的Adapter里的ViewHolder绑定问题 

	运行时不会影响APP效率,使用配置方便

	代码思路清晰,可读性强

>

---

>

### View绑定:

	onCreate里注册:
		ButerKnife.inject(this);

	Activity声明绑定控件,例如:
		@InjectView(R.id.title)
		TextView title;

	onclick等事件处理: 例如:
		@OnClick(R.id.submit) 
		public void sayHi(Button button){
			button.setText("Hello!");	
		}

>

---

>

	@InjectView(R.id.arc_progress)
	ArcProgress arcProgress;

	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		// Init
		ButterKnife.inject(this);
	}

	@OnClick(R.id.arc_progress)
	public void initProgress() {
		// ...
	}

>

---

>

用ListView展示一个列表数据，每个Item里含有一个Button，可以点击

>

	@OnItemSelected(R.id.list_view)
	void onItemSelected(int position) {
	  // TODO ...
	}

	@OnItemSelected(value = R.id.maybe_missing, callback = NOTHING_SELECTED)
	void onNothingSelected() {
	  // TODO ...
	}

>

	@Override
	public View getView(final int position, View convertView, ViewGroup parent) {
		ConfigViewHolder configViewHolder = null;
		if (convertView == null) {
		    convertView = View.inflate(context, R.layout.adapter_config, null);
		    configViewHolder = new ConfigViewHolder(convertView);
		    convertView.setTag(configViewHolder);
		} else {
		    configViewHolder = (ConfigViewHolder) convertView.getTag();
		}
		......
	} 

	// ViewHolder
	class ConfigViewHolder {

		@InjectView(R.id.item_status)
		ImageView item_status;

		@InjectView(R.id.item_description)
		TextView item_description;

		public ConfigViewHolder(View view) {
		    ButterKnife.inject(this, view);
		}

	}

>

---

>

### 在Fragegment中使用, 注解框架

>

	public class FancyFragment extends Fragment {
	  @InjectView(R.id.button1) Button button1;
	  @InjectView(R.id.button2) Button button2;

	  @Override View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
	    View view = inflater.inflate(R.layout.fancy_fragment, container, false);
	    ButterKnife.inject(this, view);
	    // TODO Use "injected" views...
	    return view;
	  }
	}

	@Override void onDestroyView() {
		super.onDestroyView();
		ButterKnife.reset(this);
	}

>

---

>

	By default, both @InjectView and listener injections are required. An exception will be thrown if the target view cannot be found.

	To suppress this behavior and create an optional injection, add the @Optional annotation to the field or method.

	@Optional @InjectView(R.id.might_not_be_there) TextView mightNotBeThere;

	@Optional @OnClick(R.id.maybe_missing) void onMaybeMissingClicked() {
	  // TODO ...
	}

>

	Custom views can bind to their own listeners by not specifying an ID.

	public class FancyButton extends Button {
	  @OnClick
	  public void onClick() {
	    // TODO do something!
	  }
	}

>

---

>

### 补充 

>

butterknife-7.0.1 发布，有些使用方法变更

>

	class ExampleActivity extends Activity {
	  @Bind(R.id.title) TextView title;
	  @Bind(R.id.subtitle) TextView subtitle;
	  @Bind(R.id.footer) TextView footer;

	  @Override public void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.simple_activity);
		ButterKnife.bind(this);
		// TODO Use fields...
	  }
	}

	public void bind(ExampleActivity activity) {
	  activity.subtitle = (android.widget.TextView) activity.findViewById(2130968578);
	  activity.footer = (android.widget.TextView) activity.findViewById(2130968579);
	  activity.title = (android.widget.TextView) activity.findViewById(2130968577);
	}

	public class FancyFragment extends Fragment {
	  @Bind(R.id.button1) Button button1;
	  @Bind(R.id.button2) Button button2;

	  @Override public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
		View view = inflater.inflate(R.layout.fancy_fragment, container, false);
		ButterKnife.bind(this, view);
		// TODO Use fields...
		return view;
	  }
	}

	public class MyAdapter extends BaseAdapter {
	  @Override public View getView(int position, View view, ViewGroup parent) {
		ViewHolder holder;
		if (view != null) {
		  holder = (ViewHolder) view.getTag();
		} else {
		  view = inflater.inflate(R.layout.whatever, parent, false);
		  holder = new ViewHolder(view);
		  view.setTag(holder);
		}

		holder.name.setText("John Doe");
		// etc...

		return view;
	  }

	  static class ViewHolder {
		@Bind(R.id.title) TextView name;
		@Bind(R.id.job_title) TextView jobTitle;

		public ViewHolder(View view) {
		  ButterKnife.bind(this, view);
		}
	  }
	}


