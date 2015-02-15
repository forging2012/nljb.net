---
title: Qt之焦点事件
date: '2015-02-15'
description:
categories:

tags:qt

---

1、setFocusPolicy（...）设置获得焦点的方式:

	Qt::TabFocus
	// 通过Tab键获得焦点
	Qt::ClickFocus
	// 通过被单击获得焦点
	Qt::StrongFocus
	// 可通过上面两种方式获得焦点
	Qt::NoFocus
	// 不能通过上两种方式获得焦点(默认值), setFocus仍可使其获得焦点
	 
2、setFocus使Widget获得焦点

	this.setFocus
 
3、void QWidget::setFocusProxy(QWidget * w)设置焦点的委托:

	将该widget的focus proxy设置给w。
	如果w为0，该函数将此widget设为没有任何focus proxy。
	有些widget，比如QComboBox，可以“拥有focus”
	但是它们会创建一个子的widget来实际地处理焦点。
	比如QComboBox创建的叫做QLineEdit。
	setFocusProxy()用来指定当该widget获得焦点时实际上由谁来处理这个焦点。
	如果某个widget拥有focus proxy，focusPolicy()，setFocusPolicy()，setFocus()和hasFocus()都是对focus proxy进行操作。

