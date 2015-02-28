---
title: Qt中拖拽移动控件位置
date: '2015-02-28'
description:
categories:

tags:qt

---

<img src="{{urls.media}}/Qt中拖拽移动控件位置/move.jpeg" alt="" width="200">

---

注：摘自网络，亲测可行.

---

	//Widget.h
	#ifndef WIDGET_H
	#define WIDGET_H
	#include <QtGui/QWidget>
	#include <QLabel>
	class Widget : public QWidget
	{
	    Q_OBJECT
	public:
	    Widget(QWidget *parent = 0);
	protected slots:
	    bool eventFilter(QObject *, QEvent *);
	private:
	    QLabel *label;
	};
	#endif // WIDGET_H

	//Widget.cpp
	#include <QEvent>
	#include <QLabel>
	#include <QMouseEvent>
	#include "Widget.h"

	// 构造
	Widget::Widget(QWidget *parent):QWidget(parent)
	{
	    label=new QLabel("hello",this);
	    // 事件扑捉
	    label->installEventFilter(this);
	}

	// 事件
	bool Widget::eventFilter(QObject *, QEvent *evt)
	{
	    static QPoint lastPnt;
	    static bool isHover = false;
	    // 鼠标按下
	    if(evt->type() == QEvent::MouseButtonPress)
	    {
		QMouseEvent* e = static_cast<QMouseEvent*>(evt);
		if(label->rect().contains(e->pos()) && //is the mouse is clicking the key
		    (e->button() == Qt::LeftButton)) //if the mouse click the right key
		{
		    lastPnt = e->pos();
		    isHover = true;
		}
	    }
	    // 鼠标移动
	    else if(evt->type() == QEvent::MouseMove && isHover)
	    {
		// 鼠标位置
		QMouseEvent* e = static_cast<QMouseEvent*>(evt);
		int dx = e->pos().x() - lastPnt.x();
		int dy=e->pos().y()-lastPnt.y();
		// 修改对象位置
		label->move(label->x()+dx,label->y()+dy);
	    }else if(evt->type() == QEvent::MouseButtonRelease && isHover)
	    {
		isHover = false;
	    }
	    return false;
	}

	//main.cpp
	#include "Widget.h"
	#include <QtGui/QApplication>

	int main(int argc, char *argv[])
	{
	    QApplication a(argc, argv);
	    Widget w;
	    w.show();
	    return a.exec();
	}
