---
title: Golang-绘图基础-绘制简单图形
date: '2014-07-09'
description:
categories:

tags:golang

---

上一节的例子效果是通过设置每一个点的的RGBA属性来实现的,这是最基础的方式，通过这种方式我们可以绘制任意形状的图形。

1、设置点的颜色一个简单例子：

效果如下：

<img src="{{urls.media}}/Golang-绘图基础-绘制简单图形/1_0.jpg" alt="" width="140" height="240">

代码如下，跟最初我们的代码唯一不同的是设置点颜色时，多了一个条件判断语句：if x%8 == 0 ，

代码如下，这种情况下，其实我们通过算法简单的实现了画垂直线的效果：

	package main

	import (
	    "fmt"
	    "image"
	    "image/color"
	    "image/png"
	    "log"
	    "os"
	)

	func main() {
	    const (
		dx = 300
		dy = 500
	    )

	    // 需要保存的文件
	    imgcounter := 123
	    imgfile, _ := os.Create(fmt.Sprintf("%03d.png", imgcounter))
	    defer imgfile.Close()

	    // 新建一个 指定大小的 RGBA位图
	    img := image.NewNRGBA(image.Rect(0, 0, dx, dy))

	    for y := 0; y < dy; y++ {
		for x := 0; x < dx; x++ {

		    if x%8 == 0 {
			// 设置某个点的颜色，依次是 RGBA
			img.Set(x, y, color.RGBA{uint8(x % 256), uint8(y % 256), 0, 255})
		    }
		}
	    }

	    // 以PNG格式保存文件
	    err := png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }

	}

比如下面一个函数就是简单的画水平线的代码函数。

	// 画 水平线
	func (img *Image) drawHorizLine(color color.Color, fromX, toX, y int) {
	    // 遍历画每个点
	    for x := fromX; x <= toX; x++ {
		img.Set(x, y, color)
	    }
	}

2、划线

Golang 官方库没有提供划线的库，不过既然有了画点的方法，我们就可以根据一套算法画出点

下面的效果和代码是按照 Bresenham's line algorithm 算法画的线。

http://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm 

这个算法画的线简单可以用下面图来标示：

<img src="{{urls.media}}/Golang-绘图基础-绘制简单图形/2_0.jpg" alt="" width="240" height="120">

下面演示代码画出来的效果图如下：

注意，为了便于看到效果， 图的左右都画了一条竖线，斜线是按照上面算法画出来的。

<img src="{{urls.media}}/Golang-绘图基础-绘制简单图形/3_0.jpg" alt="" width="140" height="240">

相关代码如下： 

这里的代码借鉴了下面的代码。 

https://github.com/akavel/polyclip-go/blob/9b07bdd6e0a784f7e5d9321bff03425ab3a98beb/polyutil/draw.go

	package main

	import (
	    "fmt"
	    "image"
	    "image/color"
	    "image/png"
	    "log"
	    "os"
	)

	// Putpixel describes a function expected to draw a point on a bitmap at (x, y) coordinates.
	type Putpixel func(x, y int)

	// 求绝对值
	func abs(x int) int {
	    if x >= 0 {
		return x
	    }
	    return -x
	}

	// Bresenham's algorithm, http://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm
	// https://github.com/akavel/polyclip-go/blob/9b07bdd6e0a784f7e5d9321bff03425ab3a98beb/polyutil/draw.go
	// TODO: handle int overflow etc.
	func drawline(x0, y0, x1, y1 int, brush Putpixel) {
	    dx := abs(x1 - x0)
	    dy := abs(y1 - y0)
	    sx, sy := 1, 1
	    if x0 >= x1 {
		sx = -1
	    }
	    if y0 >= y1 {
		sy = -1
	    }
	    err := dx - dy

	    for {
		brush(x0, y0)
		if x0 == x1 && y0 == y1 {
		    return
		}
		e2 := err * 2
		if e2 > -dy {
		    err -= dy
		    x0 += sx
		}
		if e2 < dx {
		    err += dx
		    y0 += sy
		}
	    }
	}

	func main() {

	    const (
		dx = 300
		dy = 500
	    )

	    // 需要保存的文件

	    // 新建一个 指定大小的 RGBA位图
	    img := image.NewNRGBA(image.Rect(0, 0, dx, dy))

	    drawline(5, 5, dx-8, dy-10, func(x, y int) {
		img.Set(x, y, color.RGBA{uint8(x), uint8(y), 0, 255})
	    })

	    // 左右都画一条竖线
	    for i := 0; i < dy; i++ {
		img.Set(0, i, color.Black)
		img.Set(dx-1, i, color.Black)
	    }

	    imgcounter := 250
	    imgfile, _ := os.Create(fmt.Sprintf("%03d.png", imgcounter))
	    defer imgfile.Close()

	    // 以PNG格式保存文件
	    err := png.Encode(imgfile, img)
	    if err != nil {
		log.Fatal(err)
	    }
	}

3、特殊图形
这次绘制出来的图形效果如下：

<img src="{{urls.media}}/Golang-绘图基础-绘制简单图形/4_0.jpg" alt="" width="240" height="240">

相关代码如下： 

这里的代码借鉴了下面的代码： 

https://github.com/xMachinae/pallinda13/blob/master/uppg2.go

	package main
	 
	 import (
	     "fmt"
	     "image"
	     "image/png"
	     "log"
	     "os"
	 )
	 
	 func Pic(dx, dy int) [][]uint8 {
	     pic := make([][]uint8, dx)
	     for i := range pic {
		 pic[i] = make([]uint8, dy)
		 for j := range pic[i] {
		     pic[i][j] = uint8(i * j)
		 }
	     }
	     return pic
	 }
	 
	 func main() {
	     Show(Pic)
	 }
	 
	 func Show(f func(int, int) [][]uint8) {
	     const (
		 dx = 256
		 dy = 256
	     )
	     data := f(dx, dy) // 图片坐标点的颜色二维数组。
	     m := image.NewNRGBA(image.Rect(0, 0, dx, dy))
	     for y := 0; y < dy; y++ {
		 for x := 0; x < dx; x++ {
		     v := data[y][x]
		     i := y*m.Stride + x*4
		     m.Pix[i] = v
		     m.Pix[i+1] = v
		     m.Pix[i+2] = 255
		     m.Pix[i+3] = 255
		 }
	     }
	     ShowImage(m)
	 }
	 
	 func ShowImage(m image.Image) {
	 
	     // 需要保存的文件
	     imgcounter := 1234
	     imgfile, _ := os.Create(fmt.Sprintf("%03d.png", imgcounter))
	     defer imgfile.Close()
	 
	     // 以PNG格式保存文件
	     err := png.Encode(imgfile, m)
	     if err != nil {
		 log.Fatal(err)
	     }
	 
	}

更复杂的算法

比如下面代码实现了图片简单的上下左右翻转的功能。

图片旋转的算法 

https://github.com/mpl/goexif/blob/a588a5577cedfda71e3645f8137c38495f308f6c/exif/rotate_test.go
