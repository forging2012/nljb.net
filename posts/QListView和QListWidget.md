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

---

	listWidget = QListWidget() 
	#实例化一个(itembase)的列表

	listWidget.addItem('dd') 
	#添加一个项

	listWidget.addItems([]) 
	#从序列中添加子项

	listWidget.setDragEnabled(True)
	#设置拖拉

	listWidget.sortItems() 
	#排序

	listWidget.selectAll()
	#全选

	listWidget.setSortingEnabled(bool)
	#设置自动排序

	listWidget.setSelectionMode(QtGui.QAbstractItemView.ExtendedSelection)
	#设置选择模式

	选择模式有:
	ExtendedSelection   按住ctrl多选
	SingleSelection     单选 
	MultiSelection      点击多选 
	ContiguousSelection 鼠标拖拉多选

	listWidget.setCurrentRow(0) 
	#设置当前选择行默认为-1

	listWidget.count() 
	#得到子项总数

	listWidget.item(row).text() 
	#得到第 row 行的内容 listWidget.item(row) 返回一个 item 对象

	listWidget.takeItem(row) 
	#返回row 行的所在的item 对象 可以用在 insertItem（）中

	listWidget.insertItem(2,item) 
	#在第二行插入一项item可谓为一个listviewitem对象或者string

	listWidget.setCurrentItem('dd')
	#设置'dd'为当前项

	listWidget.selectedItems() 
	#返回一个包含item对象 的list 对象

	item.setText('dsds') 
	# 设置item的内容为dsdsitem为对象 可从 listWidget.item(row) takeItem(row) 得到

---

QListWidget的用法
 
	setSelectionMode()
	#设置list一次最多可以选择多少item

	#有两种方法来listwidget中添加Item:
	#一种是在item构造时候，指定父widget，
	#如果item构造时候QListWidget已经存在，可以用下面的方法
	new QListWidgetItem(tr("Oak"), listWidget);

	#第二种方法是构造完item，在使用QListWidget::addItem()来添加item
	#向QListWidget中指定的位置插入item，使用QListWidget::insertItem(int , QListWidgetItem*)
	#使用QListWidget::count()来统计widget中总共的item数目
	#使用QListWidget::takeItem(int index)来删除表中的某一项
	#设置当前的item是第几行，QListWidget::setCurrentRow ( int row )
	#设置list是否可以自动排序QListWidget::setSortingEnabled(bool)，默认是FALSE

---

LineEdit 类

	将LineEdit框设置成密码输入框：line.setEchoMode(QLineEdit::Password)
	获取linedit的值： line.text()
	给lineedit 赋值为空： line.setText()

MessageBox 类

	实例介绍：（实现提示框）
	@kuang = Qt::MessageBox.new(Qt::MessageBox::NoIcon, "提示", "登录成功！", 0, self)
	@kuang.addButton("确定",Qt::MessageBox::AcceptRole)
	@kuang.exec()
	这样就可以创建一个提示登录成功的窗口。（注：这是在QtRuby 中的相关代码。）

TabelWidget类

	设置行表头隐藏：
	verticalHeader().setVisible(false);
	设置列表头隐藏：
	horizontalHeader().setVisible(false);
