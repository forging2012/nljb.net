---
title: Golang-Raster-字体颜色
date: '2014-07-09'
description:
categories:

tags:golang

---

	import "code.google.com/p/freetype-go/freetype/raster" 

	type RGBAPainter
	func NewRGBAPainter(m *image.RGBA) *RGBAPainter
	func (r *RGBAPainter) Paint(ss []Span, done bool)
	func (r *RGBAPainter) SetColor(c color.Color)

The raster package provides an anti-aliasing 2-D rasterizer.

It is part of the larger Freetype-Go suite of font-related packages
but the raster package is not specific to font rasterization
and can be used standalone without any other Freetype-Go package.

Rasterization is done by the same area/coverage accumulation algorithm as the Freetype "smooth" module
and the Anti-Grain Geometry library. 
A description of the area/coverage algorithm is at http://projects.tuxee.net/cl-vectors/section-the-cl-aa-algorithm

