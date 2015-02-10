---
title: Qt之QLineEdit自动补全.md
date: '2015-02-10'
description:
categories:

tags:qt

---

	// .H
	public:
	    void sendKeyword();

	protected:
	    bool eventFilter(QObject *obj, QEvent *ev);

	// .CPP
	// ui->display 存储全部内容，关键字在内容中搜索
	// 如果有多个结果，会直接输出到 ui->display 中.
	// QWidget 初始化 ui->command 是个 QLineEdit
	ui->command->installEventFilter(this);

	// 按键事件捕捉
	bool Examples::eventFilter(QObject *obj, QEvent *event)
	{
	    if (obj == ui->command) {
		if (event->type() == QEvent::KeyPress) {
		    QKeyEvent *keyEvent = static_cast<QKeyEvent*>(event);
		    if (keyEvent->key() == Qt::Key_Tab)
		    {
			// 这里触发
			sendKeyword();
		    }
		    else
			return QMainWindow::eventFilter(obj, event);
		    return true;
		} else
		    return false;
	    } else
		// pass the event on to the parent class
		return QMainWindow::eventFilter(obj, event);
	}

	// sendKeyword
	void Examples::sendKeyword()
	{
	    if(ui->command->text().isEmpty())
		return;
	    QMap<QString,int> backs;
	    QStringList keys = ui->command->text().trimmed().split(" ");
	    QStringList values = ui->display->toPlainText().split("\n");
	    for (int i = 0; i < values.size(); ++i)
	    {
		if (values.at(i).split(" ").last().trimmed().startsWith(keys.last().trimmed()))
		    backs.insert(values.at(i).split(" ").last().trimmed(), 0);
	    }
	    if (backs.size() == 0)
		return;
	    else if (backs.size() == 1)
	    {
		QString keyword;
		QMap<QString, int>::iterator it;
		for (it = backs.begin(); it != backs.end(); ++it)
		{
		    for (int i = 0; i < keys.size(); ++i)
		    {
			if (i == (keys.size() - 1))
			    keyword = keyword + " " + it.key();
			else
			    keyword = keyword + " " + keys.at(i);
		    }
		    ui->command->setText(keyword);
		}
	    }
	    else if (backs.size() > 1)
	    {
		QString command;
		command.append("<pre>");
		QMap<QString, int>::iterator it;
		for (it = backs.begin(); it != backs.end(); ++it)
		{
		    command += it.key() + " ";
		}
		command.append("</pre>");
		ui->display->append(command);
		ui->display->moveCursor(QTextCursor::End);
		}
	}
