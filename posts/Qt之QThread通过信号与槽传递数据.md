---
title: Qt之QThread通过信号与槽传递数据
date: '2015-02-11'
description:
categories:

tags:qt

---

	// 子线程 .H
	#ifndef MESSAGE_H
	#define MESSAGE_H

	#include <QThread>
	#include <QString>

	class Message : public QThread
	{
	    Q_OBJECT

	public:
	    Message();

	public:
	    void stop();

	protected:
	    void run();

	public:
	    Cgo *cgo;

	// 这里，声明一个信号
	signals:
	    void send_message_signal(const QString &);

	public slots:

	private:
	    volatile bool stopped;

	};

	#endif // MESSAGE_H

	// 子线程 .CPP
	#include <QMessageBox>
	#include <QDebug>
	#include "message.h"

	Message::Message()
	{
	    stopped = false;
	}

	void Message::run()
	{
	    // 发送信号到主线程
	    emit send_message_signal(command);
	}

	void Message::stop()
	{
	    stopped = true;
	}

---

	// 主线程 .H
	// 这里，声明一个槽
	private slots:
	    void on_message_signal(const QString &);

	// 主线程 .CPP
	// 构造, 初始化线程
	Message *message = new Message();
	// 这里，将信号与槽建立连接
	connect(message, SIGNAL(send_message_signal(const QString&)), this, SLOT(on_message_signal(const QString&)));
	// 启动线程
	message->start();

	// 接收，信号发来的数据
	void Examples::on_message_signal(const QString &_message)
	{
	    QMessageBox::information(this, tr("提示"), tr(_message.toLatin1().data()), QMessageBox::Yes);
	}
