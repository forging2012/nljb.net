---
title: Android之Context的理解及使用
date: '2015-05-05'
description:
categories:

tags:android

---

>

### Context

>

	Context在android中的作用不言而喻，当我们访问当前应用的资源，启动一个新的activity的时候

	都需要提供Context而这个Context到底是什么呢，这个问题好像很好回答又好像难以说清楚。

	从字面意思，Context的意思是“上下文”，或者也可以叫做环境、场景等，尽管如此，还是有点抽象。

	从类的继承来说，Context作为一个抽象的基类，它的实现子类有三种：Application、Activity和Service 

	那么这三种有没有区别呢？为什么通过任意的Context访问资源都得到的是同一套资源呢？

	getApplication和getApplicationContext有什么区别呢？应用中到底有多少个Context呢？

>

---

>

<img src="{{urls.media}}/Android之Context的理解及使用/1.gif" alt="" width="600">

>

---

>

### 什么是Context

>

	Context是一个抽象基类，我们通过它访问当前包的资源（getResources、getAssets）和

	启动其他组件（Activity、Service、Broadcast）以及得到各种服务（getSystemService）

	当然，通过Context能得到的不仅仅只有上述这些内容。

>

	对Context的理解可以来说：Context提供了一个应用的运行环境，在Context的大环境里，应用才可以访问资源

	才能完成和其他组件、服务的交互，Context定义了一套基本的功能接口，我们可以理解为一套规范

	而Activity和Service是实现这套规范的子类，这么说也许并不准确

	因为这套规范实际是被ContextImpl类统一实现的，Activity和Service只是继承并有选择性地重写了某些规范的实现。

>

---

>

### 可以看到 Activity 继承 Context 向上转型，获取父类对象

>

	// Activity
	public class MainActivity extends Activity {

	    // 标签
	    public String ContextLabel = "Hello World"

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			// FragmentTransaction
			replaceFragment(R.id.main, new MainFragment());
	    }
	}

	// Fragment
	public class MainFragment extends Fragment {

	    // MainActivity
	    private MainActivity activity = null;

	    // 构造	
	    public MainFragment() {
			// 实际情况
			activity = (MainActivity) getActivity()
	    }

	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			// 这样就可以获取父类对象内容了
			// activity.ContextLabel
	    }
	}

	// BaseAdapter
	class EvolverBaseAdapter extends BaseAdapter {

	    // 上下文
	    private Context context;

	    // Activity
	    public MainActivity activity = null;

	    // 构造
	    public EvolverBaseAdapter(Context context, ArrayList<EvolverData> evolver) {
			this.context = context;
			activity = (MainActivity) context;
	    }
	}

>

---

>

	// 定义了一个 TextView 并且传入 MainActivity.this
	// 由于 MainActivity.this 是继承 Activity 继承 Context
	// 所以 传入 MainActivity.this 就是传入 Context
	TextView text = new TextView(MainActivity.this);

	// 设置内容，则是通过 Context 找到 getResources 设置
	text.setText(R.string.app_name);
	
	// TextView 构造
	public TextView(Context context) {
		this(context, null);
	}

	// TextView setText 
	@android.view.RemotableViewMethod
	public final void setText(int resid) {
		setText(getContext().getResources().getText(resid));
	}



