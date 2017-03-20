---
title: Qt之QDialog与QWidget实现模态及非模态
date: '2017-03-20'
description:
categories:

tags:qt

---

>

#### Qt之QDialog与QWidget实现模态及非模态

>

***模态与非模态***

>

	模态对话框工作状态：当它获得焦点时，将垄断用户的输入，在完成本对话框之前，用户无法对本程序的其他部分进行操作	
	非模态对话框类似于WORD里的查找替换，就在应用程序打开非模态对话框的同时还可以切换到其他窗口进行操作

>


---

>

***对于 QDialog 的模态及非模态是直接可以实现的***

>

	// 一，模态QDialog
	QDialog dlg(this);
	dlg.exec();

	// 二，模态QDialog
	QDialog *pDlg=new QDialog(this);
	pDlg->setModal(true);
	pDlg->show();

	// 三，非模态QDialog
	QDialog *pDlg=new QDialog(this);
	pDlg->show();

>

---

>

***QDialog实现模态非模态很简单，但是对于QWidget有点迷茫，QWidget中没有exec()，也没有setModal()方式***

>

	// 那QWidget应该如何实现模态与非模态呢
	setWindowModality()，此函数就是用来设置QWidget运行时的程序阻塞方式的

>

* Qt::NonModal		不阻塞
* Qt::WindowModal	阻塞父窗口，所有祖先窗口及其子窗口
* Qt::ApplicationModal	阻塞整个应用程序

>

***看来，setModal()也就是使用setWindowModality()设置Qt::ApplicationModal参数也实现的模态。***

>

......

>
