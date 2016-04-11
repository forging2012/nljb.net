---
title: Golang-读取文件信息-FileInfo-and-MD5
date: '2014-07-04'
description:
categories:

tags:golang

---

	package main
	import "os"
	import "fmt"
	import "crypto/md5"
	import "io"

	func main() {

	    fi,err:= os.Lstat("Time.go") 
	    if err != nil {
		fmt.Println("info ERROR",err) 
	    }
	    fileHandle,err := os.Open("Time.go")
	    if err != nil {
		fmt.Println("open ERROR",err) 
	    }
	    defer fileHandle.Close()
	    h := md5.New()
	    _, err = io.Copy(h,fileHandle)
	    fmt.Println(fi.Name())
	    fmt.Println(fi.Size())
	    //fmt.Println(fi.Mode().Perm())
	    //fmt.Println(fi.ModTime())
	    //fmt.Println(fi.IsDir())
	    fmt.Printf("%x", h.Sum(nil))

	}


