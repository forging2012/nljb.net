---
title: Golang-字体-FreeType-go
date: '2014-07-09'
description:
categories:

tags:

---

FreeType库（http://www.freetype.org/）是一个完全免费(开源)的、高质量的且可移植的字体引擎

它提供统一的接口来访问多种字体格式文件，包括TrueType, OpenType, Type1, CID, CFF, Windows FON/FNT, X11 PCF等。

支持单色位图、反走样位图的渲染。

freetype-go就是用go语言实现了FreeType驱动。

它的项目地址： https://code.google.com/p/freetype-go

下面是使用它绘制的一个字体效果图：

<img src="{{urls.media}}/Golang-字体-FreeType-go/4bed2e738bd4b31cd3dc1d1f85d6277f9f2ff8b6.jpg" alt="" width="100" height="80">

相关代码:

	package main

	import (
	    "code.google.com/p/freetype-go/freetype"
	    "fmt"
	    "image"
	    "image/color"
	    "image/png"
	    "io/ioutil"
	    "log"
	    "os"
	)

	const (
	    dx       = 100         // 图片的大小 宽度
	    dy       = 40          // 图片的大小 高度
	    fontFile = "RAVIE.TTF" // 需要使用的字体文件
	    fontSize = 20          // 字体尺寸
	    fontDPI  = 72          // 屏幕每英寸的分辨率
	)
	 
	func main() {
	 
	    // 需要保存的文件
	    imgcounter := 123
	    imgfile, _ := os.Create(fmt.Sprintf("%03d.png", imgcounter))
	    defer imgfile.Close()
	 
	    // 新建一个 指定大小的 RGBA位图
	    img := image.NewNRGBA(image.Rect(0, 0, dx, dy))
	 
	    // 画背景
	    for y := 0; y < dy; y++ {
		for x := 0; x < dx; x++ {
		    // 设置某个点的颜色，依次是 RGBA
		    img.Set(x, y, color.RGBA{uint8(x), uint8(y), 0, 255})
		}
	    }
	 
	    // 读字体数据
	    fontBytes, err := ioutil.ReadFile(fontFile)
	    if err != nil {
		log.Println(err)
		return
	    }
	    font, err := freetype.ParseFont(fontBytes)
	    if err != nil {
		log.Println(err)
		return
	    }
	 
	    c := freetype.NewContext()
	    c.SetDPI(fontDPI)
	    c.SetFont(font)
	    c.SetFontSize(fontSize)
	    c.SetClip(img.Bounds())
	    c.SetDst(img)
	    c.SetSrc(image.White)

	    pt := freetype.Pt(10, 10+int(c.PointToFix32(fontSize)>>8)) // 字出现的位置

	    _, err = c.DrawString("ABCDE", pt)
	    if err != nil {
		log.Println(err)
		return
	    }
	 
	     // 以PNG格式保存文件
	     err = png.Encode(imgfile, img)
	     if err != nil {
		 log.Fatal(err)
	     }
	 
	 }


