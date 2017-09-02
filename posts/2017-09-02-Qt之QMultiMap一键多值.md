---
title: Qt之QMultiMap一键多值
date: '2017-09-02'
description:
categories:

tags:qt

---

>

### Qt之QMultiMap一键多值

>

***在QT中常用的键值对QMap，当遇到一键多值需要存储时往往QMap<T, vector<T>>这样存储, 但是QT提供了QMultiMap来解决这个问题*** 

>

	http://doc.qt.io/qt-4.8/qmultimap.html

>

	QMultiMap<QString, int> map1, map2, map3;

	map1.insert("plenty", 100);
	map1.insert("plenty", 2000);
	// map1.size() == 2

	map2.insert("plenty", 5000);
	// map2.size() == 1

	map3 = map1 + map2;
	// map3.size() == 3

>

	QList<int> values = map.values("plenty");
	for (int i = 0; i < values.size(); ++i)
	    cout << values.at(i) << endl;

>

	QMultiMap<QString, int>::iterator i = map.find("plenty");
	while (i != map.end() && i.key() == "plenty") {
	    cout << i.value() << endl;
	    ++i;
	}

>
