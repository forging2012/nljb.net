---
title: Golang-旋转-拉伸-放大-缩小-图片
date: '2014-08-12'
description:
categories:

tags:golang

---

	// 本例引用z库,引用graphics库
	// z "github.com/nutzam/zgo"
	// "code.google.com/p/graphics-go/graphics"

	// 打开图片
	src, err := z.ImageJPEG(file)
	// 打开失败
	if err != nil {
	    // 踢出
	    return err
	}

	// 创建一张图片
	dst := z.ImageRGBA(1920x1080);
	if dst == nil {
	    // 异常返回
	    return fmt.Errorf("image rgba fail")
	}

---
旋转图片

// 注意 SnapshotRotate 为选择角度

<img src="{{urls.media}}/Golang-旋转-拉伸-放大-缩小-图片/1.jpg" alt="" width="240">

	// 旋转图片(从src到dst)
	if err := graphics.Rotate(dst, src, &graphics.RotateOptions{SnapshotRotate}); err != nil {
		// 返回错误
		return err
	}

---
拉伸,放大,缩小图片

	// 将src图片,缩小,放大,拉伸到dst图片大小
	if err := graphics.Scale(dst, src); err != nil {
		return nil, err
	}

---
保存图片

	// 保存,具体参数请参考z库,(0-100)为图片质量
	// 也可以使用 z.ImageEncodePNG ...
	if err := z.ImageEncodeJPEG("/dev/shm/snap.jpg", dst, 50); err != nil {
		return nil, err
	}	
