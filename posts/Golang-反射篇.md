---
title: Golang-反射篇
date: '2014-07-10'
description:
categories:

tags:

---

Go语言的基本语法的使用已经在前几篇陆陆续续学完了，

下面可能想写一些Go的标准库的使用了。

先是reflect库。

reflect库的godoc在http://golang.org/pkg/reflect/

------------------------------------------------------------------------------------------

Type和Value

首先，reflect包有两个数据类型我们必须知道，一个是Type，一个是Value。

Type就是定义的类型的一个数据类型，Value是值的类型

具体的Type和Value里面包含的方法就要看文档了：

http://golang.org/pkg/reflect/

	package main
	  
	import(
	    "fmt"
	    "reflect"
	)
	  
	type MyStruct struct{
	    name string
	}
	  
	func (this *MyStruct)GetName() string {
	    return this.name
	}
	  
	func main() {
	    s := "this is string"
	    fmt.Println(reflect.TypeOf(s))
	    fmt.Println("-------------------")
	      
	    fmt.Println(reflect.ValueOf(s))
	    var x float64 = 3.4
	    fmt.Println(reflect.ValueOf(x))
	    fmt.Println("-------------------")
	      
	    a := new(MyStruct)
	    a.name = "yejianfeng"
	    typ := reflect.TypeOf(a)
	  
	    fmt.Println(typ.NumMethod())
	    fmt.Println("-------------------")
	      
	    b := reflect.ValueOf(a).MethodByName("GetName").Call([]reflect.Value{})
	    fmt.Println(b[0])
	  
	}


输出结果：

<img src="{{urls.media}}/Golang-反射篇/1_0.jpg" alt="" width="300">

这个程序看到几点：

1 TypeOf和ValueOf是获取Type和Value的方法

2 ValueOf返回的<float64 Value>是为了说明这里的value是float643 第三个b的定义实现了php中的string->method的方法，

为什么返回的是reflect.Value[]数组呢？当然是因为Go的函数可以返回多个值的原因了。

Value的方法和属性

<img src="{{urls.media}}/Golang-反射篇/2.jpg" alt="" width="300">

好了，我们看到Value的Type定义了这么多Set方法：

下面看这么个例子：

	package main
	  
	import(
	    "fmt"
	    "reflect"
	)
	  
	type MyStruct struct{
	    name string
	}
	  
	func (this *MyStruct)GetName() string {
	    return this.name
	}
	  
	func main() {
	    fmt.Println("--------------")
	    var a MyStruct
	    b := new(MyStruct)
	    fmt.Println(reflect.ValueOf(a))
	    fmt.Println(reflect.ValueOf(b))
	      
	    fmt.Println("--------------")
	    a.name = "yejianfeng"
	    b.name = "yejianfeng"
	    val := reflect.ValueOf(a).FieldByName("name")
	  
	    //painc: val := reflect.ValueOf(b).FieldByName("name")
	    fmt.Println(val)
	  
	    fmt.Println("--------------")
	    fmt.Println(reflect.ValueOf(a).FieldByName("name").CanSet())
	    fmt.Println(reflect.ValueOf(&(a.name)).Elem().CanSet())
	      
	    fmt.Println("--------------")
	    var c string = "yejianfeng"
	    p := reflect.ValueOf(&c)
	    fmt.Println(p.CanSet())   //false
	    fmt.Println(p.Elem().CanSet())  //true
	    p.Elem().SetString("newName")
	    fmt.Println(c)
	}

返回：

<img src="{{urls.media}}/Golang-反射篇/3.jpg" alt="" width="300">

这段代码能有一些事情值得琢磨：

1 为什么a和b的ValueOf返回的是不一样的？

	a是一个结构，b是一个指针。好吧，在Go中，指针的定义和C中是一样的。

2 reflect.ValueOf(a).FieldByName("name")

	这是一个绕路的写法，其实和a.name是一样的意思，主要是要说明一下Value.FieldByName的用法

3 val := reflect.ValueOf(b).FieldByName("name") 是有error的，为什么？

	b是一个指针，指针的ValueOf返回的是指针的Type，它是没有Field的，所以也就不能使用FieldByName

4 fmt.Println(reflect.ValueOf(a).FieldByName("name").CanSet())为什么是false?

看文档中的解释：

好吧，什么是addressable，and was not obtained by the use of unexported struct fields?

CanSet当Value是可寻址的时候，返回true，否则返回false

看到第二个c和p的例子，我们可以这么理解：

当前面的CanSet是一个指针的时候（p）它是不可寻址的，但是当是p.Elem()(实际上就是*p)，它就是可以寻址的

这个确实有点绕。
 
总而言之，reflect包是开发过程中几乎必备的包之一。能合理和熟练使用它对开发有很大的帮助
