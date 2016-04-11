---
title: Arduino Hello World
date: '2014-08-14'
description:
categories:

tags:arduino

---

Hello World 作为所有编程语言的起始，占据着无法改变的地位。

“Hello, world"程序是指在计算机屏幕上输出“Hello,world”这行字符串的计算机程序

“hello, world”的中文意思是“世界，你好”。

在本教程中，计算机就是Arduino uno控制板，屏幕就是串口监视器。

需要的元器件

Arduino开发板,USB线

<img src="{{urls.media}}/Arduino-Hello-World/1.png" alt="" width="438" height="162">

输入代码

	void setup()
	{
		Serial.begin(9600);
		// 设置波特率为9600，这里要跟软件设置相一致。
		// 当接入特定设备（如：蓝牙）时，我们也要跟其他设备的波特率达到一致。
	}
	void loop()
	{
		Serial.println("Hello World!");
		//显示“Hello World！”字符串
		delay(5000);
		// 延迟5秒
	}

---

代码回顾

<img src="{{urls.media}}/Arduino-Hello-World/2.png" alt="" width="400" height="400">

打开Arduino IDE，笔者是在Ubuntu上做的测试，其实在哪个操作系统下没什么太多区别

有一点需要注意的是Arduino板连接的是哪个电脑接口，当你把Arduino板插上电脑USB口后，Arduino IDE自动识别到COM口。

在Tools-Serial Port中选择识别到的接口。

把程序拷贝粘贴在IDE中，点击Verify进行编译，验证程序是否正确

这时程序还没有被写入Arduino板，然后点击upload，可以看到Arduino板LED闪烁几下，IDE提示Done uploading。

完成上传后，点击Tools-Serial Monitor：

<img src="{{urls.media}}/Arduino-Hello-World/3.png" alt="" width="400" height="400">

显示输出“Hello,world！”

<img src="{{urls.media}}/Arduino-Hello-World/4.jpg" alt="" width="371" height="230">

---

代码进阶

你也可以做些交互，例如当你在Serial Monitor中输入“h”时，才会输出“Hello,world！”。

代码如下：

	int val;
	//定义变量val
	int ledpin=13;
	//定义数字接口13

	void setup()
	{
		Serial.begin(9600);
		// 设置波特率为9600，这里要跟软件设置相一致。
		// 当接入特定设备（如：蓝牙）时，我们也要跟其他设备的波特率达到一致。
		pinMode(ledpin,OUTPUT);
		// 设置数字13 口为输出接口，Arduino 上我们用到的I/O 口都要进行类似这样的定义。
	}

	void loop()
	{
		val=Serial.read();
		// 读取PC 机发送给Arduino 的指令或字符，并将该指令或字符赋给val
		if(val=='h')
		// 判断接收到的指令或字符是否是“h”。
		{
		// 如果接收到的是“h”字符
			digitalWrite(ledpin,HIGH);
			// 点亮数字13 口LED。
			delay(500);
			// 延迟5毫秒
			digitalWrite(ledpin,LOW);
			// 熄灭数字13 口LED
			delay(500);
			Serial.println("Hello World!");
			// 显示“Hello World！”字符串
		}
	}

