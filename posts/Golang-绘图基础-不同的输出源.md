---
title: Golang-绘图基础-不同的输出源
date: '2014-07-09'
description:
categories:

tags:golang

---

先看一个简单代码， 它执行后会产生下面的300*500的png图片文件：

<img src="{{urls.media}}/Golang-绘图基础-不同的输出源/1_0.jpg" alt="" width="140" height="240">

代码：

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
		     // 设置某个点的颜色，依次是 RGBA
		     img.Set(x, y, color.RGBA{uint8(x % 256), uint8(y % 256), 0, 255})
		 }
	     }
	 
	     // 以PNG格式保存文件
	     err := png.Encode(imgfile, img)
	     if err != nil {
		 log.Fatal(err)
	     }
	 
	}

我们再看一个代码，以http文件流的方式展示图片，效果如下:

<img src="{{urls.media}}/Golang-绘图基础-不同的输出源/2_0.jpg" alt="" width="140" height="240">

代码如下：

	package main

	import (
	    "image"
	    "image/color"
	    "image/png"
	    "net/http"
	    "time"
	)

	func pic(w http.ResponseWriter, req *http.Request) {
	    const (
		dx = 300
		dy = 500
	    )

	    // 新建一个 指定大小的 RGBA位图
	    img := image.NewNRGBA(image.Rect(0, 0, dx, dy))

	    for y := 0; y < dy; y++ {
		for x := 0; x < dx; x++ {
		    // 设置某个点的颜色，依次是 RGBA
		    img.Set(x, y, color.RGBA{uint8(x % 256), uint8(y % 256), 0, 255})
		}
	    }

	    // 图片流方式输出
	    w.Header().Set("Content-Type", "image/png")
	    png.Encode(w, img)
	}

	func main() {
	    http.HandleFunc("/", pic)
	    s := &http.Server{
		Addr:           ":82",
		ReadTimeout:    30 * time.Second,
		WriteTimeout:   30 * time.Second,
		MaxHeaderBytes: 1 << 20}
	    s.ListenAndServe()

	}

通过两种方式来展示图片,我们可以看到,上面两个代码最大的区别就是输出源不一样,

一个是到文件,一个是到http流. 而我们的代码也很清晰的展示了这两种方式只需要修改对应的实现即可.其他代码是没有变化的.

这里代码中的 image.NewNRGBA 返回的 image对象是一个内存中的图片。
