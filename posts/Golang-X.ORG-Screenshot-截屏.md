---
title: Golang-X.ORG-Screenshot-截屏
date: '2014-08-08'
description:
categories:

tags:golang

---

	// 所谓 x-go-binding 也就是 x-c-binding 的 GO 版本

	import (
		"github.com/BurntSushi/xgb"
		"github.com/BurntSushi/xgb/xproto"
		"image"
	)

	// 截图
	func CaptureScreen() (*image.RGBA, error) {

		// 屏幕连接
		var c *xgb.Conn

		// 判断是否连接到了屏幕
		if sl.xgbConn == nil {
			// 建立连接
			conn, err := xgb.NewConn()
			// 判断是否连接成功
			if err != nil {
				return nil, err
			}
			// 成功保存全局
			sl.xgbConn = conn
			// 赋值
			c = conn
		} else {
			// 已经连接
			c = sl.xgbConn
		}

		screen := xproto.Setup(c).DefaultScreen(c)

		rect := image.Rect(0, 0, 
			int(screen.WidthInPixels), int(screen.HeightInPixels))

		// ------------------------------------------ //

		x, y := rect.Dx(), rect.Dy()
		xImg, err := xproto.GetImage(c, 
				xproto.ImageFormatZPixmap,
				xproto.Drawable(screen.Root), 
				int16(rect.Min.X),
			 	int16(rect.Min.Y), 
				uint16(x), 
				uint16(y), 
				0xffffffff).Reply()
		if err != nil {
			return nil, err
		}

		data := xImg.Data
		for i := 0; i < len(data); i += 4 {
			data[i], data[i+2], data[i+3] = data[i+2], data[i], 255
		}

		img := &image.RGBA{data, 4 * x, image.Rect(0, 0, x, y)}
		return img, nil
	}

---

	// 生成 image.RGBA 数据
	// 想要保存为文件参考Z库方法
	// z "github.com/nutzam/zgo"
	
	// JPEG将编码生成图片
	// 选择编码参数,质量范围从1到100,更高的是更好 &jpeg.Options{90}
	func ImageEncodeJPEG(ph string, img image.Image, option int) error {
		// 确保文件父目录存在
		FcheckParents(ph)
		// 打开文件等待写入
		f := FileW(ph)
		// 保证文件正常关闭
		defer f.Close()
		// 写入文件
		return jpeg.Encode(f, img, &jpeg.Options{option})
	}

	// PNG将编码生成图片
	func ImageEncodePNG(ph string, img image.Image) error {
		// 确保文件父目录存在
		FcheckParents(ph)
		// 打开文件等待写入
		f := FileW(ph)
		// 保证文件正常关闭
		defer f.Close()
		// 写入文件
		return png.Encode(f, img)
	}

