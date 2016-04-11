---
title: Golang-压缩-解压-Tar.Gz
date: '2014-07-04'
description:
categories:

tags:golang

---

golang处理压缩包,我最常用的就是tar.gz了,所以今天写了一个测试一下.代码放这里以后浏览.

	//压缩文件
	package main
	  
	import (
	    "fmt"
	    "os"
	    "io"
	    "archive/tar"
	    "compress/gzip"
	)
	  
	func main() {
	    // file write
	    fw, err := os.Create("tar/lin_golang_src.tar.gz")
	    if err != nil {
		panic(err)
	    }
	    defer fw.Close()
	  
	    // gzip write
	    gw := gzip.NewWriter(fw)
	    defer gw.Close()
	  
	    // tar write
	    tw := tar.NewWriter(gw)
	    defer tw.Close()
	  
	    // 打开文件夹
	    dir, err := os.Open("file/")
	    if err != nil {
		panic(nil)
	    }
	    defer dir.Close()
	  
	    // 读取文件列表
	    fis, err := dir.Readdir(0)
	    if err != nil {
		panic(err)
	    }
	  
	    // 遍历文件列表
	    for _, fi := range fis {
		// 逃过文件夹, 我这里就不递归了
		if fi.IsDir() {
		    continue
		}
	  
		// 打印文件名称
		fmt.Println(fi.Name())
	  
		// 打开文件
		fr, err := os.Open(dir.Name() + "/" + fi.Name())
		if err != nil {
		    panic(err)
		}
		defer fr.Close()
	  
		// 信息头
		h := new(tar.Header)
		h.Name = fi.Name()
		h.Size = fi.Size()
		h.Mode = int64(fi.Mode())
		h.ModTime = fi.ModTime()
	  
		// 写信息头
		err = tw.WriteHeader(h)
		if err != nil {
		    panic(err)
		}
	  
		// 写文件
		_, err = io.Copy(tw, fr)
		if err != nil {
		    panic(err)
		}
	    }
	  
	    fmt.Println("tar.gz ok")
	}
	解压文件
	package main
	  
	import (
	    "fmt"
	    "os"
	    "io"
	    // "time"
	    "archive/tar"
	    "compress/gzip"
	)
	  
	func main() {
	    // file read
	    fr, err := os.Open("tar/lin_golang_src.tar.gz")
	    if err != nil {
		panic(err)
	    }
	    defer fr.Close()
	  
	    // gzip read
	    gr, err := gzip.NewReader(fr)
	    if err != nil {
		panic(err)
	    }
	    defer gr.Close()
	  
	    // tar read
	    tr := tar.NewReader(gr)
	  
	    // 读取文件
	    for {
		h, err := tr.Next()
		if err == io.EOF {
		    break
		}
		if err != nil {
		    panic(err)
		}
	  
		// 显示文件
		fmt.Println(h.Name)
	  
		// 打开文件
		fw, err := os.OpenFile("file2/" + h.Name, os.O_CREATE | os.O_WRONLY, 0644/*os.FileMode(h.Mode)*/)
		if err != nil {
		    panic(err)
		}
		defer fw.Close()
	  
		// 写文件
		_, err = io.Copy(fw, tr)
		if err != nil {
		    panic(err)
		}
		  
	    }
	  
	    fmt.Println("un tar.gz ok")
	}

呼呼,以后打包下载东西的时候可以使用了.

http://www.dotcoo.com/golang-tar-gzip

--------------------------------------------------------------------------------

	// 解压Tar文件
	func Untar(file,pathstring) error {
	    // 打开文件
	    f,err:=os.Open(file)
	    iferr!= nil {
		returnerr
	    }
	    deferf.Close()
	    // 读取GZIP
	    gr,err:=gzip.NewReader(f)
	    iferr!= nil {
		returnerr
	    }
	    defergr.Close()
	    // 读取TAR
	    tr:=tar.NewReader(gr)
	    for {
		hdr,err:=tr.Next()
		iferr==io.EOF{
		    break
		} else iferr!= nil {
		    returnerr
		}
		ifhdr.FileInfo().IsDir() {
		    os.MkdirAll(path+string(os.PathSeparator)+hdr.Name,hdr.FileInfo().Mode())
		} else {
		    fw,err:=os.OpenFile(path+string(os.PathSeparator)+hdr.Name,os.O_CREATE|os.O_WRONLY|os.O_TRUNC,hdr.FileInfo().Mode())
		    iferr!= nil {
			returnerr
		    }
		    deferfw.Close()
		    _,err=io.Copy(fw,tr)
		    iferr!= nil {
			returnerr
		    }
		}
	    }
	    return nil
	}


