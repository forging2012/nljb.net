---
title: Qt动画框架之QPropertyAnimation类
date: '2015-03-04'
description:
categories:

tags:qt

---

### 从下向上，或者从上向下弹出一个 Dialog 窗口 

>

	Dialog::Dialog(QWidget *parent) :
	    QDialog(parent),
	    ui(new Ui::Dialog)
	{
	    ui->setupUi(this);
	    // 隐藏菜单栏
	    // this->setWindowFlags(Qt::FramelessWindowHint);
	    // 初始化位置到右下角 (初始位置, 不一定需要设置）
	    // this->move((desktop.availableGeometry().width()-this->width()),desktop.availableGeometry().height());
	    // 开始显示右下角弹出框
	    showAnimation();
	}


	Dialog::~Dialog()
	{
	    delete ui;
	}

	// 弹出动画
	void Dialog::showAnimation(){
	    // 显示弹出框动画
	    animation=new QPropertyAnimation(this,"pos");
	    // 移动间隔
	    animation->setDuration(2000);
	    // 移动效果	
	    animation->setEasingCurve(QEasingCurve::OutBounce);
	    // 起始位置
	    animation->setStartValue(QPoint(this->x(),-desktop.availableGeometry().height()));
	    // 终点位置
	    animation->setEndValue(QPoint((this->x()),(this->y())));
	    // 开始移动
	    animation->start();

	}

	// 关闭动画
	void Dialog::closeAnimation(){
	    connect(animation,SIGNAL(finished()),this,SLOT(clearAll()));
	}

	// 清理动画指针
	void Dialog::clearAll(){
	    disconnect(animation,SIGNAL(finished()),this,SLOT(clearAll()));
	    delete animation;
	    animation = NULL;
	}

---

### 网上的一些例子

>

	QPushButton button("Animated Button");
	button.show();
	QPropertyAnimation animation(&button, "geometry");
	animation.setDuration(10000);
	animation.setStartValue(QRect(0, 0, 100, 30));
	animation.setEndValue(QRect(250, 250, 100, 30));
	animation.start();
	// 上述代码即在10秒的期限把button从屏幕的左上角移动到(250,250)点处。
        // 上述代码在开始值与结束值之间做了线性插值。
	// 当然，设置的值在开始处与结束处之间的数值也是合理的，那么插值衍化就沿这些点进行。

	QPushButton button("Animated Button");
	button.show();
	QPropertyAnimation animation(&button, "geometry");
	animation.setDuration(10000);
	animation.setKeyValueAt(0, QRect(0, 0, 100, 30));
	animation.setKeyValueAt(0.8, QRect(250, 250, 100, 30));
	animation.setKeyValueAt(1, QRect(0, 0, 100, 30));
	animation.start();
	// 在这个例子中，在8秒的期限将button移到(250,250)，然后在剩下的2秒时间移回至初始的位置；
	// 这些点之间的移动都是通过线性插值的。

	QPushButton button("Animated Button");
	button.show();
	QPropertyAnimation animation(&button, "geometry");
	animation.setDuration(3000);
	animation.setStartValue(QRect(0, 0, 100, 30));
	animation.setEndValue(QRect(250, 250, 100, 30));
	animation.setEasingCurve(QEasingCurve::OutBounce);
	animation.start();
	// 上述代码中，动画即沿着OutBounce曲线，该曲线样式是到结束处会弹跳起来像个弹跳球。
	// QEasingCurve类有大量的供你选择的曲线，它们被定义成QEasingCurve::Type枚举。
	// 如果你需要另外的曲线样式，你也可以自己实现一个，然后用QEasingCurve注册它既可。


	QPushButton *bonnie = new QPushButton("Bonnie");
	bonnie->show();
	QPushButton *clyde = new QPushButton("Clyde");
	clyde->show();
	QPropertyAnimation *anim1 = new QPropertyAnimation(bonnie, "geometry");// Set up anim1  
	QPropertyAnimation *anim2 = new QPropertyAnimation(clyde, "geometry");// Set up anim2  
	QParallelAnimationGroup *group = new QParallelAnimationGroup;
	group->addAnimation(anim1);
	group->addAnimation(anim2);
	group->start();
	// 动画分组

	QPushButton button("Animated Button");
	button.show();
	QPropertyAnimation anim1(&button, "geometry");
	anim1.setDuration(3000);
	anim1.setStartValue(QRect(0, 0, 100, 30));
	anim1.setEndValue(QRect(500, 500, 100, 30));
	QPropertyAnimation anim2(&button, "geometry");
	anim2.setDuration(3000);
	anim2.setStartValue(QRect(500, 500, 100, 30));
	anim2.setEndValue(QRect(1000, 500, 100, 30));
	QSequentialAnimationGroup group;
	group.addAnimation(&anim1);
	group.addAnimation(&anim2);
	group.start();
	// 动画分组
