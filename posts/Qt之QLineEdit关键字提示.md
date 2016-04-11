---
title: Qt之QLineEdit关键字提示
date: '2015-02-10'
description:
categories:

tags:qt

---

<img src="{{urls.media}}/Qt之QLineEdit关键字提示/keyword.png" alt="" width="600">

>

---

	// .H
	public:
	    // history 内容
	    QStringList *history;
	    QCompleter *completer;
	    QStandardItemModel *model;

	private slots:
	    void onCommandChanged(const QString&);
	    void onCommandChoosed(const QString&);

---

	// .CPP
	// ...
	history = new QStringList();
	model = new QStandardItemModel(0, 1, this);
	completer = new QCompleter(model, this);
	// ui->command 是一个 QLineEdit
	ui->command->setCompleter(completer);
	connect(completer, SIGNAL(activated(const QString&)), this, SLOT(onCommandChoosed(const QString&)));
	// ui->command 是一个 QLineEdit
	connect(ui->command, SIGNAL(textChanged(const QString&)), this, SLOT(onCommandChanged(const QString&)));
	// ...

	// ...
	void Examples::onCommandChoosed(const QString& _command)
	{
	    // 清除已存在的文本更新内容
	    ui->command->clear();
	    ui->command->setText(_command);
	}

	void Examples::onCommandChanged(const QString& _str)
	{
	    // 清除已经存在的数据
	    model->removeRows(0, model->rowCount());

	    // 遍历所有的命令, 在内容中查找关键字开头.
	    for (int i = 0; i < history->size(); ++i)
	    {
		// 插入包含关键字的数据
		if (history->at(i).startsWith(_str))
		{
		    model->insertRow(0);
		    model->setData(model->index(0, 0), history->at(i));
		}
	    }
	}

