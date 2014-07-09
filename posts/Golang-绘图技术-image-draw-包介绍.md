---
title: Golang-绘图技术-image-draw-包介绍
date: '2014-07-09'
description:
categories:

tags:golang

---

image/draw 包仅仅定义了一个操作：通过可选的蒙版图（mask image）

把一个原始图片绘制到目标图片上，这个操作是出奇的灵活，可以优雅和高效的执行很多常见的图像处理任务。

	// Draw calls DrawMask with a nil mask.
	func Draw(dst Image, r image.Rectangle, src image.Image, sp image.Point, op Op)
	func DrawMask(dst Image, r image.Rectangle, src image.Image, sp image.Point, mask image.Image, mp image.Point, op Op)
	第一个函数Draw是没有使用蒙版mask的调用方法，它内部其实就是调用的mask为 nil的方法。

	它的参数描述如下：
	dst  绘图的背景图。
	r 是背景图的绘图区域
	src 是要绘制的图
	sp 是 src 对应的绘图开始点（绘制的大小 r变量定义了）
	mask 是绘图时用的蒙版，控制替换图片的方式。
	mp 是绘图时蒙版开始点（绘制的大小 r变量定义了）
	op Op is a Porter-Duff compositing operator.  参考文章：http://blog.csdn.net/ison81/article/details/5468763 
	Porter-Duff 等式12种规则可以看这篇博客：http://www.blogjava.net/onedaylover/archive/2008/01/16/175675.html
	 
 
下图就是几个相关的例子：

mask 蒙版是渐变

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/1.jpg" alt="" width="600">

给一个矩形填充颜色
使用 Draw方法的逻辑效果图：

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/2.jpg" alt="" width="600">

代码：

	m := image.NewRGBA(image.Rect(0, 0, 640, 480))
	blue := color.RGBA{0, 0, 255, 255}
	draw.Draw(m, m.Bounds(), &image.Uniform{blue}, image.ZP, draw.Src)

拷贝图片的一部分
效果特效如下：

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/3.jpg" alt="" width="600">

相关代码：

	r := image.Rectangle{dp, dp.Add(sr.Size())}  // 获得更换区域
	draw.Draw(dst, r, src, sr.Min, draw.Src)

	如果是复制整个图片，则更简单：

	sr = src.Bounds()         // 获取要复制图片的尺寸
	r := sr.Sub(sr.Min).Add(dp)   // 目标图的要剪切区域
	draw.Draw(dst, r, src, sr.Min, draw.Src)

图片滚动效果
效果如下图:

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/4.jpg" alt="" width="600">

假设我们需要把图片 m 上移20个像素.

相关代码:

	b := m.Bounds()
	p := image.Pt(0, 20)
	// Note that even though the second argument is b,
	// the effective rectangle is smaller due to clipping.
	draw.Draw(m, b, m, b.Min.Add(p), draw.Src)
	dirtyRect := b.Intersect(image.Rect(b.Min.X, b.Max.Y-20, b.Max.X, b.Max.Y))

把一个图片转成RGBA格式
效果图:

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/5.jpg" alt="" width="600">

相关代码:

	b := src.Bounds()
	m := image.NewRGBA(b)
	draw.Draw(m, b, src, b.Min, draw.Src)

通过蒙版画特效
效果图

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/6.jpg" alt="" width="600">

相关代码
  
	type circle struct {
	    p image.Point
	    r int
	}

	func (c *circle) ColorModel() color.Model {
	    return color.AlphaModel
	}

	func (c *circle) Bounds() image.Rectangle {
	    return image.Rect(c.p.X-c.r, c.p.Y-c.r, c.p.X+c.r, c.p.Y+c.r)
	}

	func (c *circle) At(x, y int) color.Color {
	    xx, yy, rr := float64(x-c.p.X)+0.5, float64(y-c.p.Y)+0.5, float64(c.r)
	    if xx*xx+yy*yy < rr*rr {
		return color.Alpha{255}
	    }
	    return color.Alpha{0}
	}

	draw.DrawMask(dst, dst.Bounds(), src, image.ZP, &circle{p, r}, image.ZP, draw.Over)


注意,一个image对象只需要实现下面几个就可,这也就是Go接口强大的地方.

	type Image interface {
	    // ColorModel returns the Image's color model.
	    ColorModel() color.Model
	    // Bounds returns the domain for which At can return non-zero color.
	    // The bounds do not necessarily contain the point (0, 0).
	    Bounds() Rectangle
	    // At returns the color of the pixel at (x, y).
	    // At(Bounds().Min.X, Bounds().Min.Y) returns the upper-left pixel of the grid.
	    // At(Bounds().Max.X-1, Bounds().Max.Y-1) returns the lower-right one.
	    At(x, y int) color.Color
	}

画一个字体
效果图，画一个蓝色背景的字体。

<img src="{{urls.media}}/Golang-绘图技术-image-draw-包介绍/7.jpg" alt="" width="600">

相关伪代码：

	src := &image.Uniform{color.RGBA{0, 0, 255, 255}}
	mask := theGlyphImageForAFont()
	mr := theBoundsFor(glyphIndex)
	draw.DrawMask(dst, mr.Sub(mr.Min).Add(p), src, image.ZP, mask, mr.Min, draw.Over)
	 
上面例子完整的代码请看：
http://golang.org/doc/progs/image_draw.go
 
参考：
http://blog.golang.org/go-imagedraw-package
