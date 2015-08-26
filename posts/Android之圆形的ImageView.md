---
title: Android之圆形的ImageView
date: '2015-08-26'
description:
categories:

tags:android

---

>

### Circle ImageView

>

	https://github.com/hdodenhof/CircleImageView

>

	dependencies {
		...
		compile 'de.hdodenhof:circleimageview:1.3.0'
	}

>

---

>

<img src="{{urls.media}}/Android之圆形的ImageView/1.png" alt="" width="500" hight="900">

>

---

>

    <de.hdodenhof.circleimageview.CircleImageView xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/user_logo"
        android:layout_width="96dp"
        android:layout_height="96dp"
        android:layout_below="@+id/user_title"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="30dp"
        android:src="@mipmap/ic_launcher"
        app:border_color="#FF000000"
        app:border_width="2dp" />

>
