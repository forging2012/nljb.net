---
title: Golang-官方图片库
date: '2014-07-09'
description:
categories:

tags:golang

---

	http://blog.golang.org/go-image-package

	http://blog.golang.org/go-imagedraw-package

	http://blog.golang.org/gif-decoder-exercise-in-go-interfaces

Golang 的图片出来通过提供操作每一个像素点设置颜色（http://www.cnblogs.com/ghj1976/p/3441536.html）

提供通过可选蒙版图片重叠操作 （http://www.cnblogs.com/ghj1976/p/3443638.html） 这两种基础方式

这样任何想要的效果都可以自己实现， 但是旋转、缩放等相关的图像算法也是比较麻烦的

这时候我们就需要借助官方提供的图片包处理了，图片包在：https://code.google.com/p/graphics-go

获取方法： go get code.google.com/p/graphics-go/graphics

它支持的几个效果举例：
 
图片旋转

效果:一个旋转前,一个旋转后

<img src="{{urls.media}}/Golang-官方图片库/1.jpg" alt="" width="200">

代码例子：

	package main

	import (
	    "code.google.com/p/graphics-go/graphics"
	    "fmt"
	    "image"
	    "image/png"
	    "log"
	    "os"
	)

	func main() {
	    src, err := LoadImage("348.png")
	    if err != nil {
		log.Fatal(err)
	    }

	    dst := image.NewRGBA(image.Rect(0, 0, 350, 400))

	    err = graphics.Rotate(dst, src, &graphics.RotateOptions{3.5})
	    if err != nil {
		log.Fatal(err)
	    }

	    // 需要保存的文件
	    imgcounter := 123
	    saveImage(fmt.Sprintf("%03d.png", imgcounter), dst)
	}

	// LoadImage decodes an image from a file.
	func LoadImage(path string) (img image.Image, err error) {
	    file, err := os.Open(path)
	    if err != nil {
		return
	    }
	    defer file.Close()
	    img, _, err = image.Decode(file)
	    return
	}

	// 保存Png图片
	func saveImage(path string, img image.Image) (err error) {
	    // 需要保存的文件
	    imgfile, err := os.Create(path)
	    defer imgfile.Close()

	    // 以PNG格式保存文件
	    err = png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }
	    return
	}

代码说明：

旋转的参数是顺时针旋转的弧度，弧度相关的介绍如下：(http://youthpasses.blog.51cto.com/2909834/799353)

每个角度对应的弧度可以看下面图。

<img src="{{urls.media}}/Golang-官方图片库/2.jpg" alt="" width="200">

代码参考：(http://stackoverflow.com/questions/12430874/image-manipulation-in-golang)

图片模糊处理

效果：

下面一个是清晰版本，一个是模糊出来后的版本。

<img src="{{urls.media}}/Golang-官方图片库/3.jpg" alt="" width="200">

仔细对比细节是可以看到模糊效果的。

代码：

	package main

	import (
	    "code.google.com/p/graphics-go/graphics"
	    "fmt"
	    "image"
	    "image/png"
	    "log"
	    "os"
	)

	func main() {
	    src, err := LoadImage("348.png")
	    if err != nil {
		log.Fatal(err)
	    }

	    dst := image.NewRGBA(image.Rect(0, 0, 350, 400))

	    err = graphics.Blur(dst, src, &graphics.BlurOptions{StdDev: 1.1})
	    if err != nil {
		log.Fatal(err)
	    }

	    // 需要保存的文件
	    imgcounter := 510
	    saveImage(fmt.Sprintf("%03d.png", imgcounter), dst)
	}

	// LoadImage decodes an image from a file.
	func LoadImage(path string) (img image.Image, err error) {
	    file, err := os.Open(path)
	    if err != nil {
		return
	    }
	    defer file.Close()
	    img, _, err = image.Decode(file)
	    return
	}

	// 保存Png图片
	func saveImage(path string, img image.Image) (err error) {
	    // 需要保存的文件
	    imgfile, err := os.Create(path)
	    defer imgfile.Close()

	    // 以PNG格式保存文件
	    err = png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }
	    return
	}

代码说明：
模糊参数：

	// BlurOptions are the blurring parameters.
	// StdDev is the standard deviation of the normal, higher is blurrier.
	// StdDev 是正常的标准偏差， 值越大越虚化
	// Size is the size of the kernel. If zero, it is set to Ceil(6 * StdDev).
	//
	type BlurOptions struct {
	    StdDev float64
	    Size   int
	}

缩略图
原始图:

<img src="{{urls.media}}/Golang-官方图片库/4.jpg" alt="" width="200">

确保数据完整的缩放,效果如下:

<img src="{{urls.media}}/Golang-官方图片库/5.jpg" alt="" width="20" height="80">

相关代码:

	package main

	import (
	    "code.google.com/p/graphics-go/graphics"
	    "fmt"
	    "image"
	    "image/png"
	    "log"
	    "os"
	)

	func main() {
	    src, err := LoadImage("348.png")
	    if err != nil {
		log.Fatal(err)
	    }

	    // 缩略图的大小
	    dst := image.NewRGBA(image.Rect(0, 0, 20, 80))

	    // 产生缩略图,等比例缩放
	    err = graphics.Scale(dst, src)
	    if err != nil {
		log.Fatal(err)
	    }

	    // 需要保存的文件
	    imgcounter := 734
	    saveImage(fmt.Sprintf("%03d.png", imgcounter), dst)
	}

	// LoadImage decodes an image from a file.
	func LoadImage(path string) (img image.Image, err error) {
	    file, err := os.Open(path)
	    if err != nil {
		return
	    }
	    defer file.Close()
	    img, _, err = image.Decode(file)
	    return
	}

	// 保存Png图片
	func saveImage(path string, img image.Image) (err error) {
	    // 需要保存的文件
	    imgfile, err := os.Create(path)
	    defer imgfile.Close()

	    // 以PNG格式保存文件
	    err = png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }
	    return
	}

图片数据可以丢弃的缩放效果:

<img src="{{urls.media}}/Golang-官方图片库/6.jpg" alt="" width="20" height=80>

相关代码:

	package main

	import (
	    "code.google.com/p/graphics-go/graphics"
	    "fmt"
	    "image"
	    "image/png"
	    "log"
	    "os"
	)

	func main() {
	    src, err := LoadImage("348.png")
	    if err != nil {
		log.Fatal(err)
	    }

	    // 缩略图的大小
	    dst := image.NewRGBA(image.Rect(0, 0, 20, 80))

	    // 产生缩略图
	    err = graphics.Thumbnail(dst, src)
	    if err != nil {
		log.Fatal(err)
	    }

	    // 需要保存的文件
	    imgcounter := 670
	    saveImage(fmt.Sprintf("%03d.png", imgcounter), dst)
	}

	// LoadImage decodes an image from a file.
	func LoadImage(path string) (img image.Image, err error) {
	    file, err := os.Open(path)
	    if err != nil {
		return
	    }
	    defer file.Close()
	    img, _, err = image.Decode(file)
	    return
	}

	// 保存Png图片
	func saveImage(path string, img image.Image) (err error) {
	    // 需要保存的文件
	    imgfile, err := os.Create(path)
	    defer imgfile.Close()

	    // 以PNG格式保存文件
	    err = png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }
	    return
	}
 
更多相关资料请看下面地址:

https://code.google.com/p/graphics-go/source/browse/graphics/?r=9a6eb915f43de825cd2a26c8b8866422d0a3f2ec

