---
title: Linux-GPIO
date: '2014-08-13'
description:
categories:

tags:system

---

可以使用系统中的GPIOLIB模块在用户空间提供的sysfs接口，实现应用层对GPIO的独立控制。

>

使用 Linux Kernel 提供的 sysfs 來控制 GPIO

在要寫程式之前，我們先來使用 Linux Kernel 提供的 sysfs 來控制 GPIO。

---

用户空间gpio的调用 

用户空间访问gpio,即通过sysfs接口访问gpio,下面是/sys/class/gpio目录下的文件： 

	--export/unexport文件
	--gpioN指代具体的gpio引脚
	--gpio_chipN指代gpio控制器
	// 必须知道以上接口没有标准device文件和它们的链接。 

(1) export/unexport文件接口：

        /sys/class/gpio/export，该接口只能写不能读
	// 用户程序通过写入gpio的编号来向内核申请将某个gpio的控制权导出到用户空间

	比如  echo 19 > export 
	// 上述操作会为19号gpio创建一个节点gpio19，此时目录下生成gpio19的目录
        // 和(unexport)导出的效果相反。 

        比如 echo 19 > unexport
        // 上述操作将会移除gpio19这个节点。 

(2) /sys/class/gpio/gpioN

	// 指代某个具体的gpio端口,里边有如下属性文件
	direction 表示gpio端口的方向，读取结果是in或out。
	// 该文件也可以写，写入out 时该gpio设为输出同时电平默认为低。
	// 写入low或high则不仅可以设置为输出 还可以设置输出的电平。 
	// 当然如果内核不支持或者内核代码不愿意，将不会存在这个属性
	// 比如内核调用了gpio_export(N,0)就表示内核不愿意修改gpio端口方向属性 
      
	value 表示gpio引脚的电平,0(低电平)1（高电平）
	// 如果gpio被配置为输出，这个值是可写的，记住任何非零的值都将输出高电平,
	// 如果某个引脚能并且已经被配置为中断,则可以调用poll(2)函数监听该中断,中断触发后poll(2)函数就会返回。
                                   
	edge 表示中断的触发方式，edge文件有如下四个值："none", "rising", "falling"，"both"。
        // none表示引脚为输入，不是中断引脚
        // rising表示引脚为中断输入，上升沿触发
        // falling表示引脚为中断输入，下降沿触发
        // both表示引脚为中断输入，边沿触发
        // 这个文件节点只有在引脚被配置为输入引脚的时候才存在。 当值是none时可以通过如下方法将变为中断引脚
        // echo "both" > edge;对于是both,falling还是rising依赖具体硬件的中断的触发方式。
	// 此方法即用户态gpio转换为中断引脚的方式
                
        active_low 
	// 相互调换高低电平设置
                                                                
(3) /sys/class/gpio/gpiochipN

      gpiochipN表示的就是一个gpio_chip,用来管理和控制一组gpio端口的控制器： 
      // base   和N相同，表示控制器管理的最小的端口编号。 
      // lable   诊断使用的标志（并不总是唯一的） 
      // ngpio  表示控制器管理的gpio端口数量（端口范围是：N ~ N+ngpio-1） 

---

1. 首先先將 GPIO4 設定成可以用 sysfs 控制

	echo 4 > /sys/class/gpio/export

2. 設定 GPIO4 為輸出腳

	echo out > /sys/class/gpio/gpio4/direction

3. 設定 GPIO4 輸出值為 1 (0: 低電位, 1: 高電位)

	echo 1 > /sys/class/gpio/gpio4/value

4. 設定 GPIO4 輸出值為 0 (0: 低電位, 1: 高電位)

	echo 0 > /sys/class/gpio/gpio4/value

5. 取消建立出來的 GPIO4 node

	echo 4 > /sys/class/gpio/unexport

>

在你執行以上第 3 步的時候，你可以看到 LED 亮了起來，直到第 4 步時，才又變回原本的狀態。

若想要使用 Bash 來控制 GPIO，則可以採用此種方式。

---

使用 debugfs 來觀看目前的 GPIO 設定

我們可以使用 debugfs 來察看目前的 GPIO 設定，首先掛載 debugfs

	mount -t debugfs debug /d

接著就可以使用

	cat /d/gpio

來取得目前 GPIO 的狀況

	root@raspberrypi:/home/pi#
	cat /d/gpio
	GPIOs 0-53, bcm2708_gpio:
	gpio-4  (sysfs                ) out hi

