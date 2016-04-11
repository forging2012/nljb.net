---
title: x-go-binding
date: '2014-07-03'
description:
categories:

tags:golang

---

	package main
	 
	import (
	    "code.google.com/p/x-go-binding/xgb"
	    "fmt"
	)
	 
	func main() {
	 
	    conn, err := xgb.Dial(":0")
	    if err != nil {
		panic(err)
	    }
	 
	    // X Windows Width Height
	    fmt.Println(conn.DefaultScreen().WidthInPixels)
	    fmt.Println(conn.DefaultScreen().HeightInPixels)
	 
	    // Depth 8 16 24
	    fmt.Println(conn.DefaultScreen().RootDepth)
	 
	    fmt.Println(conn.DefaultScreen().Root)
	    fmt.Println(conn.DefaultScreen().DefaultColormap)
	    fmt.Println(conn.DefaultScreen().WhitePixel)
	    fmt.Println(conn.DefaultScreen().BlackPixel)
	    fmt.Println(conn.DefaultScreen().CurrentInputMasks)
	    fmt.Println(conn.DefaultScreen().WidthInMillimeters)
	    fmt.Println(conn.DefaultScreen().HeightInMillimeters)
	    fmt.Println(conn.DefaultScreen().MinInstalledMaps)
	    fmt.Println(conn.DefaultScreen().MaxInstalledMaps)
	    fmt.Println(conn.DefaultScreen().RootVisual)
	    fmt.Println(conn.DefaultScreen().BackingStores)
	    fmt.Println(conn.DefaultScreen().SaveUnders)
	    fmt.Println(conn.DefaultScreen().AllowedDepthsLen)
	    fmt.Println(conn.DefaultScreen().AllowedDepths)
	 
	}

