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

### Adapter

>
