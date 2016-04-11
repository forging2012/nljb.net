---
title: Golang-语言文件操作
date: '2014-07-04'
description:
categories:

tags:golang

---

收集整理了一下的代码.参照着使用吧,自己做个记录.

	func Open(name string) (file *File, err error),*File 是实现了 io.Reader这个接口

	byte[] 转化为 bytes.Buffer:bytes.NewBuffer([]byte).

一、建立与打开
 
建立文件函数：
 
	func Create(name string) (file *File, err Error)
	 
	func NewFile(fd int, name string) *File
	 
具体见官网：http://golang.org/pkg/os/#Create
 
打开文件函数：
 
	func Open(name string) (file *File, err Error)
	 
	func OpenFile(name string, flag int, perm uint32) (file *File, err Error)
	 
具体见官网：http://golang.org/pkg/os/#Open
 
二、写文件
 
写文件函数：
 
	func (file *File) Write(b []byte) (n int, err Error)
	 
	func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
	 
	func (file *File) WriteString(s string) (ret int, err Error)
	 
具体见官网：http://golang.org/pkg/os/#File.Write 
 
写文件示例代码：
 
	package main
	import (
		"os"
		"fmt"
	)
	func main() {
		userFile := "test.txt"
		fout,err := os.Create(userFile)
		defer fout.Close()
		if err != nil {
			fmt.Println(userFile,err)
			return
		}
		for i:= 0;i<10;i++ {
			fout.WriteString("Just a test!\r\n")
			fout.Write([]byte("Just a test!\r\n"))
		}
	}
 
三、读文件
 
读文件函数：
 
	func (file *File) Read(b []byte) (n int, err Error)
	 
	func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
	 
具体见官网：http://golang.org/pkg/os/#File.Read
 
读文件示例代码：
 
	package main
	import (
		"os"
		"fmt"
	)
	func main() {
		userFile := "test.txt"
		fin,err := os.Open(userFile)
		defer fin.Close()
		if err != nil {
			fmt.Println(userFile,err)
			return
		}
		buf := make([]byte, 1024)
		for{
			n, _ := fin.Read(buf)
			if0 == n { break }
			os.Stdout.Write(buf[:n])
		}
	}
	 
四、删除文件
 
函数：
 
	func Remove(name string) Error

------------------------------------------------------------------------------------------------

	// 使用os库
	package main
	 
	import (
	    "io"
	    "os"
	)
	 
	func main() {
	    fi, err := os.Open("input.txt")
	    if err != nil { panic(err) }
	    defer fi.Close()
	 
	    fo, err := os.Create("output.txt")
	    if err != nil { panic(err) }
	    defer fo.Close()
	 
	    buf := make([]byte, 1024)
	    for {
		n, err := fi.Read(buf)
		if err != nil && err != io.EOF { panic(err) }
		if n == 0 { break }
	 
		if n2, err := fo.Write(buf[:n]); err != nil {
		    panic(err)
		} else if n2 != n {
		    panic("error in writing")
		}
	    }
	}

---------------

	// 这个例子使用了os.Open os.Create。
 
	// 使用bufio库
	package main
	 
	import (
	    "bufio"
	    "io"
	    "os"
	)
	 
	func main() {
	    fi, err := os.Open("input.txt")
	    if err != nil { panic(err) }
	    defer fi.Close()
	    r := bufio.NewReader(fi)
	 
	    fo, err := os.Create("output.txt")
	    if err != nil { panic(err) }
	    defer fo.Close()
	    w := bufio.NewWriter(fo)
	 
	    buf := make([]byte, 1024)
	    for {
		n, err := r.Read(buf)
		if err != nil && err != io.EOF { panic(err) }
		if n == 0 { break }
	 
		if n2, err := w.Write(buf[:n]); err != nil {
		    panic(err)
		} else if n2 != n {
		    panic("error in writing")
		}
	    }
	 
	    if err = w.Flush(); err != nil { panic(err) }
	}

-------------

	// 使用ioutil库
	package main
	 
	import (
	    "io/ioutil"
	)
	 
	func main() {
	    b, err := ioutil.ReadFile("input.txt")
	    if err != nil { panic(err) }
	 
	    err = ioutil.WriteFile("output.txt", b, 0644)
	    if err != nil { panic(err) }
	}


