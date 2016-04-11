---
title: Golang 正坐标转换为反坐标
date: '2014-08-07'
description:
categories:

tags:golang

---

	// C 为两个矩阵相交区域

	// 矩阵
	type Rect struct {
		x0 int // 左定点X
		y0 int // 左顶点Y
		x1 int // 右底点X
		y1 int // 右底点Y
	}

	func NodeChangeRect(b, c *Rect) (x0, y0, x1, y1 float32) {
		// 对象宽高
		b_width := b.x1 - b.x0
		b_height := b.y1 - b.y0
		c_width := c.x1 - c.x0
		c_height := c.y1 - c.y0
		// 计算
		x0 = float32(c.x0-b.x0) / float32(b_width)
		y0 = float32(b_height-((c.y0-b.y0)+c_height)) / float32(b_height)
		x1 = float32(c.x0-b.x0+c_width) / float32(b_width)
		y1 = float32(b_height-(c.y0-b.y0)) / float32(b_height)
		// 返回
		return
	}

