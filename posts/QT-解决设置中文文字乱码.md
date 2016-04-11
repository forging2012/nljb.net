---
title: QT-解决设置中文文字乱码
date: '2014-09-05'
description:
categories:

tags:qt

---

	#include <QtGui/QApplication>
	#include <QTextCodec>
	#include "mainwindow.h"
	 
	int main(int argc, char *argv[])
	{
	 
	    QApplication a(argc, argv);

	    // 以下部分解决中文乱码
	    QTextCodec::setCodecForTr(QTextCodec::codecForName("GB2312"));
	    QTextCodec::setCodecForLocale(QTextCodec::codecForName("GB2312"));
	    QTextCodec::setCodecForCStrings(QTextCodec::codecForName("GB2312"));
	    // 以上部分解决中文乱码

	    MainWindow w;
	 
	    w.show();
	 
	    return a.exec();
	}

