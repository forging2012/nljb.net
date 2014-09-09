---
title: QListView和QListWidget
date: '2014-09-09'
description:
categories:

tags:qt

---

QListView是基于Model，而QListWidget是基于Item。

往QListView中添加条目需借助QAbstractListModel:

	// 如：

	    MainWindow::MainWindow(QWidget *parent) :
	    QMainWindow(parent),
	    ui(new Ui::MainWindow)
	{
	    ui->setupUi(this);
	    QStringListModel* slm = new QStringListModel(this);
	    QStringList* sl = new QStringList();
	    sl->append("asdfsadfsa");
	    sl->append("asdfsadfsa");
	    sl->append("asdfsadfsa");
	    slm->setStringList(*sl);
	    ui->listView->setModel(slm);
	    delete sl;

	}

---

而在QListWidget中添加条目可以直接additem

	// 如：

	MainWindow::MainWindow(QWidget *parent) :
	    QMainWindow(parent),
	    ui(new Ui::MainWindow)
	{
	    ui->setupUi(this);
	    QStringList* sl = new QStringList();
	    sl->append("1");
	    sl->append("2");
	    sl->append("3");
	    ui->listWidget->addItems(*sl);
	    connect(ui->pushButton,SIGNAL(clicked()), this, SLOT(close()));

	}

