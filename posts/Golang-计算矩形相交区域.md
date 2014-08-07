---
title: Golang 计算矩形相交区域
date: '2014-08-07'
description:
categories:

tags:golang

---

	// 计算矩形相较区域

	// 矩阵
	type Rect struct {
		x0 int // 左定点X
		y0 int // 左顶点Y
		x1 int // 右底点X
		y1 int // 右底点Y
	}

	// 计算矩形交点
	func NodeIntersectRect(a *Rect, b *Rect) *Rect {
		// 对象
		rect := new(Rect)
		// 计算
		rect.x0 = int(math.Max(float64(a.x0), float64(b.x0)))
		rect.y0 = int(math.Max(float64(a.y0), float64(b.y0)))
		rect.x1 = int(math.Min(float64(a.x1), float64(b.x1)))
		rect.y1 = int(math.Min(float64(a.y1), float64(b.y1)))
		// 判断是否相交
		if rect.x0 >= rect.x1 || rect.y0 >= rect.y1 {
			// 不相交
			return nil
		}
		// 相交区域
		return rect
	}


