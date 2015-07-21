---
title: Android之各分辨率定义的图片规格
date: '2015-07-21'
description:
categories:

tags:android

---

>

各种规格总结

>

---

>

应用程序图标 （Icon）应当是一个 Alpha 通道透明的32位 PNG 图片。

>

	由于安卓设备众多，一个应用程序图标需要设计几种不同大小，如：
	LDPI (Low Density Screen，120 DPI)，其图标大小为 36 x 36 px。
	MDPI (Medium Density Screen, 160 DPI)，其图标大小为 48 x 48 px。
	HDPI (High Density Screen, 240 DPI)，其图标大小为 72 x 72 px。
	xhdpi (Extra-high density screen, 320 DPI)，其图标大小为 96 x 96 px。

	建议在设计过程中，在四周空出几个像素点使得设计的图标与其他图标在视觉上一致，例如，
	96 x 96 px 图标可以画图区域大小可以设为 88 x 88 px， 四周留出4个像素用于填充（无底色）。
	72 x 72 px 图标可以画图区域大小可以设为 68 x 68 px， 四周留出2个像素用于填充（无底色）。
	48 x 48 px 图标可以画图区域大小可以设为 46 x 46 px， 四周留出1个像素用于填充（无底色）。
	36 x 36 px 图标可以画图区域大小可以设为 34 x 34 px， 四周留出1个像素用于填充（无底色）。

>

---

>

	每英寸像素数, 可以反映屏幕的清晰度，用于缩放UI的
	ldpi (low) ~120dpi
	mdpi (medium) ~160dpi
	hdpi (high) ~240dpi
	xhdpi (extra-high) ~320dpi
	xxhdpi (extra-extra-high) ~480dpi
	xxxhdpi (extra-extra-extra-high) ~640dpi

>

---

>

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/1.gif" alt="" width="500" hight="300" >

<img src="{{urls.media}}/Android之各分辨率定义的图片规格/2.gif" alt="" width="600" hight="250" >

---

>

	drawable-ldpi	放低分辨率的图片，即QVGA(240×320)
	drawable-mdpi	放中分辨率的图片，即HVGA(320×480)
	drawable-hdpi	放高分辨率的图片，如WVGA(480x800),FWVGA (480x854)。
	drawable-xhdpi	放高分辨率的图片，即720p(1280×720)
	drawable-xxhdpi	放高分辨率的图片，即1080p(1920×1080)

>

---

>


