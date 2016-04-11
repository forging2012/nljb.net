---
title: Golang-GoSerial
date: '2014-07-04'
description:
categories:

tags:golang

---


<img src="{{urls.media}}/Golang-GoSerial/e824b899a9014c08f85495470a7b02087af4f4e1.jpg" alt="" width="600">

https://github.com/tarm/goserial 

需要预先通过go get github.com/tarm/goserial获取git地址

然后通过,下载

	git clone https://github.com/tarm/goserial /root/go/src/pkg/github.com/tarm/goserial

	package main

	import "fmt"
	import "github.com/tarm/goserial"

	func main(){

	    c := &serial.Config{Name: "/dev/ttyUSB0", Baud: 115200}
	    s, err := serial.OpenPort(c)
	    if err != nil {
		fmt.Println(err)
	    }
	    n, err := s.Write([]byte("ATZ+CSQ\r"))
	    if err != nil {
		fmt.Println(err)
	    }
	    buf := make([]byte, 2048)
	    n, err = s.Read(buf)
	    if err != nil {
		fmt.Println(err)
	    }
	    fmt.Printf("%q", buf[:n])
	}

最后 go build 编译即可，需要安装GCC
