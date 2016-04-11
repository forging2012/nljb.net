---
title: Golang-文件操作
date: '2014-07-15'
description:
categories:

tags:golang

---

Go语言，在Package io中，定义了io.Reader和io.Writer两个interface

Go以组合、委派等面向对象的方式，让其它的对象定义具体的实现

如果对一个文件每次读取一行(Readline)的话，就可以使用bufio.ReadString来实现，下边是一个简单的例子：

	////////////////////////////////源代码////////////////////////////////
	package main

	import (
	   "os"
	   "bufio"
	   "fmt"
	)

	func main(){   
	    //打开文件，并进行相关处理
	   f , err := os.Open("test.txt",os.O_RDONLY,0)
	   if err != nil{
	       fmt.Printf("%v\n",err)
	       os.Exit(1)
	   }
	   //文件关闭
	   defer f.Close() 
	    
	    //将文件作为一个io.Reader对象进行buffered I/O操作
	   br := bufio.NewReader(f)
	   for{
	       //每次读取一行
	       line , err := br.ReadString('\n')
	       if err == os.EOF {
		   break
	       }else{
		   fmt.Printf("%v",line)
	       }
	   }
	}
	////////////////////////////////源代码////////////////////////////////


两个包具有文件操作的相关方法，一个是os,一个是syscall,其中os中的相关方法是对syscall相关方法的封装，推荐使用os中的相关方法。

文件的打开文件的第一步操作实际上是创建，但是由于文件的打开方法也可以创建，实际中使用创建方法的地方不多。文件打开有两个方法：

	//以只读方式打开一个存在的文件，打开就可以读取了。
	func Open(name string) (file *File, err error)

	//以各种方式打开各种存在不存在的文件，具体怎么样看flag和perm。
	func OpenFile(name string, flag int, perm FileMode) (file *File, err error)

---

flag可选值（掩码）：

	const (
	    O_RDONLY int = syscall.O_RDONLY // open the file read-only.
	    O_WRONLY int = syscall.O_WRONLY // open the file write-only.
	    O_RDWR   int = syscall.O_RDWR   // open the file read-write.
	    O_APPEND int = syscall.O_APPEND // 在文件末尾追加，打开后cursor在文件结尾位置
	    O_CREATE int = syscall.O_CREAT  // 如果不存在则创建
	    O_EXCL   int = syscall.O_EXCL   //与O_CREATE一起用，构成一个新建文件的功能，它要求文件必须不存在
	    O_SYNC   int = syscall.O_SYNC   // 同步方式打开，没有缓存，这样写入内容直接写入硬盘，系统掉电文件内容有一定保证
	    O_TRUNC  int = syscall.O_TRUNC  // 打开并清空文件
	)

perm是文件的unix权限位，可以直接用数字写，如0644。可选值有：

	const (
	    // The single letters are the abbreviations
	    // used by the String method's formatting.
	    ModeDir        FileMode = 1 << (32 - 1 - iota) // d: is a directory
	    ModeAppend                                     // a: append-only
	    ModeExclusive                                  // l: exclusive use
	    ModeTemporary                                  // T: temporary file (not backed up)
	    ModeSymlink                                    // L: symbolic link
	    ModeDevice                                     // D: device file
	    ModeNamedPipe                                  // p: named pipe (FIFO)
	    ModeSocket                                     // S: Unix domain socket
	    ModeSetuid                                     // u: setuid
	    ModeSetgid                                     // g: setgid
	    ModeCharDevice                                 // c: Unix character device, when ModeDevice is set
	    ModeSticky

	    // Mask for the type bits. For regular files, none will be set.
	    ModeType = ModeDir | ModeSymlink | ModeNamedPipe | ModeSocket | ModeDevice

	    ModePerm FileMode = 0777 // permission bits
	)

上述可选值是权限位的高有效位，低有效位的值还是要用户自己写数字。

文件的写入

	func (f *File) Write(b []byte) (n int, err error)

// 该函数写入len(b)个字节。如果返回值值n!=len(b)，则表明没写进去，err将!=nil。
// 可能有人会疑惑，write函数为什么不指定写入的个数呢？C语言的对应函数就是指定的。
// 其实go不一样啦，因为go有slice，如果你想写入8个字节，你可以Write(b)。

	func (f *File) WriteAt(b []byte, off int64) (n int, err error) //WriteAt实际上是省略了seek的步骤。

示例代码（该代码只写入http)：package main

	import (
	    "os"
	)

	func main() {
	    fd,_:=os.OpenFile("a.txt",os.O_RDWR|os.O_CREATE,0644)
	    buf:=[]byte("http://www.usr.cc")
	    fd.Write(buf)
	    fd.Close()
	}

	// 从光标的当前位置开始读len(b)个字节，返回值n是实际上读到的字节数。
	// 读取操作可能遇到EOF而停止，此时计数n为０，err为io.EOF.
	文件的读取func (f *File) Read(b []byte) (n int, err error)

	// 这个不多说了。
	func (f *File) ReadAt(b []byte, off int64) (n int, err error)

下面的示例代码写入http://www.usr.cc，读到的是http：package main

	import (
	    "os"
	    "fmt"
	)

	func main() {
	    fd,_:=os.OpenFile("a.txt",os.O_RDWR|os.O_CREATE,0644)
	    buf:=[]byte("http://www.usr.cc")
	    fd.Write(buf)
	    rx_buf:=make([]byte,4)
	    fd.ReadAt(rx_buf,0)
	    fmt.Println(string(rx_buf))
	    fd.Close()
	}

	// 文件的关闭fb.Close()

写程序离不了文件操作，这里总结下go语言文件操作。

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
	import(
		"os"
		"fmt"
		)

	func main() {
		userFile := "test.txt"
		fout,err := os.Create(userFile)
		deferfout.Close()
		if err != nil{
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
	import("os"
	"fmt"
	)
	func main() {
	    userFile := "test.txt"
	    fin,err := os.Open(userFile)
	    deferfin.Close()
	    if err != nil{
		fmt.Println(userFile,err)
		return
	    }
	    buf := make([]byte, 1024)
	    for{
		  n, _ := fin.Read(buf)
		 if0== n { break}os.Stdout.Write(buf[:n])
		}
	}

四、删除文件

函数：

func Remove(name string) Error


