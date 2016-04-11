---
title: QT-简单类及方法
date: '2014-09-05'
description:
categories:

tags:qt

---

	#include <QApplication>
	#include <QTextCodec>
	#include <QLabel>
	#include <QPushButton>
	#include <QWidget>
	#include <QSpinBox>
	#include <QSlider>
	#include <QHBoxLayout>

	int main(int argc, char *argv[])
	{

	    // -------------------------------- //
	    // 解决中文字体乱码问题

	    QTextCodec::setCodecForTr(QTextCodec::codecForName("UTF-8"));
	    QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF-8"));
	    QTextCodec::setCodecForCStrings(QTextCodec::codecForName("UTF-8"));

	    // -------------------------------- //
	    // APP框架，用来管理整个应用程序资源

	    QApplication app(argc, argv);

	    // -------------------------------- //
	    // 按钮事件

	    // QPushButton * button = new QPushButton("QUIT");
	    // QObject::connect(button, SIGNAL(clicked()), &app, SLOT(quit()));
	    // button->show();

	    // -------------------------------- //

	    // 窗口部件
	    QWidget *window = new QWidget;
	    // 设置标题
	    window->setWindowTitle("Enter Your Age");

	    // 微调框
	    QSpinBox * spinBox = new QSpinBox;
	    spinBox->setRange(0, 130);

	    // 滑动条
	    QSlider * slider = new QSlider(Qt::Horizontal);
	    slider->setRange(0, 130);

	    // 连接触发微调框修改滑框
	    QObject::connect(spinBox, SIGNAL(valueChanged(int)),
			     slider, SLOT(setValue(int)));

	    // 连接触发滑动条修改微调框
	    QObject::connect(slider, SIGNAL(valueChanged(int)),
			     spinBox, SLOT(setValue(int)));

	    // 设置滑动条值
	    slider->setValue(35);

	    // 设置滑动条值
	    spinBox->setValue(35);

	    // 竖直布局
	    // QVBoxLayout *layout = new QVBoxLayout;
	    // 把窗口排列在一个网格中
	    // QGridLayout *layout = new QGridLayout;
	    // 水平布局
	    QHBoxLayout * layout = new QHBoxLayout;

	    // 向布局增加组建
	    layout->addWidget(spinBox);
	    layout->addWidget(slider);

	    // 将布局设置给窗口
	    window->setLayout(layout);
	    window->show();

	    // -------------------------------- //

	    return app.exec();
	}


