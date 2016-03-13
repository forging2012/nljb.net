---
title: IOS之屏幕适配与SizeClass
date: '2016-03-13'
description:
categories:

tags:ios

---

>

### 屏幕适配 与 Size Class

>

<img src="{{urls.media}}/IOS之屏幕适配与SizeClass/1.png" alt="" width="450" height=600">

>

* Size Class 面板是一个九宫格，可以组合出9种不同的布局
* Size Class 九宫格中 Width 和 Height 两个布局方向，坐标原点在左上角 
* Size Class Width 和 Height 布局方向上还有三个类别：紧凑，任意，正常
* // 紧凑(Compact)，任意(Any)，正常(Regular)
* // 例如：iPhone竖屏的时候，水平方向是紧凑，垂直方向是正常
* // 例如：iPhone横屏的时候，水平方向是正常，垂直方向是紧凑

>

***IOS的三中分辨率***

* 资源分辨率，也就是资源图片的大小，单位是像素
* 设计分辨率，逻辑上的屏幕大小，单位是点
* 屏幕分辨率，以像素为单位的屏幕大小

>

***IOS的屏幕分辨率***

| 设备 | 尺寸（英寸）| 像素 | 说明 |
| --- | :---: | :---: | :---: |
| iPhone 6 Plus | 5.5 | 1920 x 1080 | Retina HD 高清显示屏, 401PPI |
| iPhone 6 | 4.7 | 1334 x 750 | Retina HD 高清显示屏, 326PPI |
| iPhone 5/5S/5C(iPod touch 5) | 4 | 1136 x 640 | Retina 显示屏, 326PPI |
| iPhone 4S | 3.5 | 960 x 640 | Retina 显示屏, 326PPI |
| iPad Air | 9.7 | 2048 x 1536 | Retina 显示屏, 264PPI |
| iPad3 | 9.7 | 2048 x 1536 | Retina 显示屏, 264PPI |
| iPad2 | 9.7 | 1024 x 768 | 普通显示屏, 163PPI |
| iPad mini 2 | 7.9 | 2048 x 1536 | Retina 显示屏, 326PPI |
| iPad mini | 9.7 | 1024 x 768 | 普通显示屏, 163PPI |

>

***IOS的设备分辨率***

| 设备 | 资源分辨率(像素) | 设计分辨率(点) | 屏幕分辨率(像素) | 说明 |
| --- | :---: | :---: | :---: | :---: |
| iPhone 6 Plus | 2208 x 1242 | 736 x 414 | 1920 x 1080 | 1 = x3 - x1.15 |
| iPhone 6 | 1334 x 750 | 667 x 375 | 1334 x 750 | 1 = x2 |
| iPhone 5/5s/5c(iPod touch 5) | 1136 x 640 | 568 x 320 | 1136 x 640 | 1 = x2 |
| iPhone 4s | 960 x 640 | 480 x 320 | 960 x 640 | 1 = x2 |
| iPad Air | 2048 x 1536 | 1024 x 768 | 2048 x 1536 | 1 = x2 |
| iPad3 | 2048 x 1536 | 1024 x 768 | 2048 x 1536 | 1 = x2 |
| iPad2 | 1024 x 768 | 1024 x 768 | 1024 x 768 | 1 = x1 |
| iPad mini 2 | 2048 x 1536 | 1024 x 768 | 2048 x 1536 | 1 = x2 |
| iPad mini | 1024 x 768 | 1024 x 768 | 1024 x 768 | 1 = x1 |

>

***这9种组合用来解决所有的IOS多屏幕适配***

* wCompact|hCompact 适用于 3.5/4 和 4.7 英寸的 iPhone 横屏情形
* wAny|hCompact 适用于所有的垂直方向是 紧凑 的性情，例如：iPhone 横屏
* wRegular|hCompact 适用于 5.5 英寸的 iPhone 横屏情形
* wCompact|hAny 适用于所有的水平方向是 紧凑 的情形，例如：3.5/4 和 4.7 英寸 iPhone 竖屏情形
* wAny|hAny 适用于所有的布局情形，但这种情形是最后的选择
* wRegular|hAny 适用于所有的水平方向是 正常 的情形，例如：iPad 的横屏和竖屏
* wCompact|hRegular 适用于所有的 iPhone 竖屏情形
* wAny|hRegular 适用于所有的垂直方向是 正常 的情形，例如：iPhone 竖屏、iPad 横屏和竖屏
* wRegular|hRegular 适用于所有的 iPad 横屏和竖屏情形

>