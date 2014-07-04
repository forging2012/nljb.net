---
title: Golang-纳秒级别-局域网-广播-时间同步
date: '2014-07-04'
description:
categories:

tags:golang

---

	package main

	import "fmt"
	import "net"
	import "os/exec"
	import "time"

	func TimeRead(ch chan string) {
	    ch <- "LOG >>> TimeRead Start"
	    socket, err := net.ListenUDP("udp4",&net.UDPAddr{
		IP: net.IPv4(0,0,0,0),
		Port: 8888,
	    })
	    if err != nil {
		fmt.Println("Err >>> TimeRead",err)
	    }
	    for {
		data := make([]byte, 32)
		//read, remoteAddr, err := socket.ReadFromUDP(data)
		read, _, err := socket.ReadFromUDP(data)
		if err != nil {
		    fmt.Println("Err >>> TimeRead",err)
		    continue
		}
		//fmt.Println(read, remoteAddr)
		cmd := exec.Command("date","-s",string(data[0:read]))
		err = cmd.Run()
		if err != nil {
		    fmt.Println("Err >>> TimeRead",err)
		}
		fmt.Println("LOG >>> TimeRead",time.Now().String())
	    }
	    ch <- "LOG >>> TimeRead Down"
	    defer socket.Close()
	}

	func Command(ch chan string) {
	    ch <- "LOG >>> Command Start"
	    socket, err := net.ListenUDP("udp4",&net.UDPAddr{
		IP: net.IPv4(0,0,0,0),
		Port: 9999,
	    })
	    if err != nil {
		fmt.Println("Err >>> Command",err)
	    }
	    for {
		data := make([]byte, 32)
		//read, remoteAddr, err := socket.ReadFromUDP(data)
		read, _, err := socket.ReadFromUDP(data)
		if err != nil {
		    fmt.Println("Err >>> Command",err)
		    continue
		}
		//fmt.Println(read, remoteAddr)
		ch <- string(data[0:read])
	    }
	    ch <- "LOG >>> Command Down"
	    defer socket.Close()
	}

	func main() {
	    timeread := make(chan string)
	    command := make(chan string)
	    go TimeRead(timeread)
	    go Command(command)
	    for {
		select {
		    case x := <- timeread:
			fmt.Println(x)
		    case y := <- command:
			fmt.Println(y)
		}
	    }
	}

//////////////////////////////////////////////////////////////////////////////////////////////////

	package main

	import "fmt"
	import "net"
	import "time"

	func TimeWrite() {
	    socket, err := net.DialUDP("udp4", nil, &net.UDPAddr{
		IP: net.IPv4(192,168,24,255),
		Port: 8888,
	    })
	    if err != nil {
		fmt.Println("Err",err)
	    }
	    defer socket.Close()

	    for {
		senddata := []byte(time.Now().Format("01/02/2006 15:04:05.999999999"))
		_, err = socket.Write(senddata)
		if err != nil {
		    fmt.Println("Err", err)
		}
		time.Sleep(time.Second * 5)
	    }
	}

	func Command() {
	    socket, err := net.DialUDP("udp4", nil, &net.UDPAddr{
		IP: net.IPv4(192,168,24,255),
		Port: 9999,
	    })
	    if err != nil {
		fmt.Println("Err",err)
	    }
	    defer socket.Close()

	    for {
		senddata := []byte("reboot")
		_, err = socket.Write(senddata)
		if err != nil {
		    fmt.Println("Err", err)
		}
		time.Sleep(time.Second * 5)
	    }
	}

	func main() {
	   go TimeWrite()
	   go Command()
	   time.Sleep(time.Second * 99999999)
	}


