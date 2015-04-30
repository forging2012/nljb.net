---
title: Android之FragmentManager介绍
date: '2015-04-30'
description:
categories:

tags:android

---

>

### FragmentManager

>

为了管理Activity中的fragments，需要使用FragmentManager.

>

为了得到它，需要调用Activity中的getFragmentManager()方法。

>

---

>

因为FragmentManager的API是在Android 3.0，也即API level 11开始引入的

>

所以对于之前的版本，需要使用support library中的FragmentActivity，并且使用getSupportFragmentManager()方法。

>

---

>

### FragmentManager 可以做的工作有：

>

	可以得到Activity中存在的fragment：

	可以使用findFragmentById()或findFragmentByTag()方法。

	可以将fragment弹出back stack(popBackStack)

	可以将back stack中最后一次的fragment转换弹出, 没有可以出栈时则返回false。

	可以为back stack加上监听器：addOnBackStackChangedListener()

>

### Performing Fragment Transactions

>

	使用Fragment时，可以通过用户交互来执行一些动作，比如增加、移除、替换等。

	所有这些改变构成一个集合，这个集合被叫做一个transaction, 通过FragmentTransaction来处理

	可以将transaction存进由activity管理的back stack中，这样就可以进行fragment变化的回退操作。

	可以这样得到FragmentTransaction类的实例：　

	FragmentManager fragmentManager = getFragmentManager();
	FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

>

### Transaction 是一组同时执行的变化的集合

>

	用add(), remove(), replace()方法，把所有需要的变化加进去，然后调用commit()方法，将这些变化应用。

	在commit()方法之前，你可以调用addToBackStack()，把这个transaction加入back stack中去

	这个back stack是由activity管理的，当用户按返回键时，就会回到上一个fragment的状态。

	比如下面的代码就是用一个新的fragment取代之前的fragment，并且将前次的状态存储在back stack中。

	// Create new fragment and transaction
	Fragment newFragment = new ExampleFragment();
	FragmentTransaction transaction = getFragmentManager().beginTransaction();

	// Replace whatever is in the fragment_container view with this fragment,
	// and add the transaction to the back stack
	transaction.replace(R.id.fragment_container, newFragment);
	transaction.addToBackStack(null);

	// Commit the transaction
	transaction.commit();

	// R.id.fragment_container
	<FrameLayout
		android:id="@+id/fragment_container"
		android:layout_width="match_parent"
		android:layout_height="match_parent">
	</FrameLayout>

>

	在这个例子中，newFragment将取代在R.id.fragment_container容器中的fragment

		如果没有，将直接添加新的fragment。

	通过调用addToBackStack()，commit()的一系列转换作为一个transaction被存储在back stack中

		用户按Back键可以返回上一个转换前的状态。

	当你移除一个fragment的时候:

		如果commit()之前没有调用 addToBackStack()，那个fragment将会是destroyed；

		如果调用了addToBackStack()，这个fragment 会是stopped，可以通过返回键来恢复。

>

### 关于commit()方法

	调用commit()方法并不能立即执行transaction中包含的改变动作，commit()方法把transaction加入activity的UI线程队列中。

	但是，如果觉得有必要的话，可以调用executePendingTransactions()方法来立即执行commit()提供的transaction。

	这样做通常是没有必要的，除非这个transaction被其他线程依赖。

	注意：你只能在activity存储它的状态（当用户要离开activity时）之前调用commit()，在存储状态之后调用将会抛出一个异常。

	这是因为当activity再次被恢复时commit之后的状态将丢失。如果丢失也没关系，那么使用commitAllowingStateLoss()方法。

 
