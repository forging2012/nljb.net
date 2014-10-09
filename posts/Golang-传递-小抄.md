---
title: Golang 传递 小抄
date: '2014-07-03'
description:
categories:

tags:golang

---

> 一些学习总结

> 1、GoLang里面interface类型式一切类型的基类型，一个函数的参数如果始inteface{}

> 说明可以接受一切类型，只要这个类型中包含需要的那个方法，调用时候就不会失败；
    
	func test(i interface{}){
	    i.Get()
	}

> 2、方法定义中可以制定某个类型（或者指针）是其调用者，方法的返回可以按照名称返回；

	func (p *A) test(i int){
	  
	}

	func test()(p int){
	    p:=1
	    return
	}

> 3、switch流程可以强制穿透功能；
>
> 4、语意上对并发的支持，用go关键词；
>
> 5、make关键词只能创建channel,数组类型；其它对象的创建用new关键词；
>
> 6、方法内用new关键词创建的对象（指针）可以返回，用&标记也可以返回本地指针；
>
> 7、方法参数如果是数字，除了类型相同外，大小必须明确，否则视为slice类型；
>
> 数组传参是按照值传递，map、slice按照引用传递；
>
> 8、数组，slice，map结构的遍历用range实现；
>
> 9、不支持指针地址的++操作；
>
> 10、type  A struct{} 、type B A 、type  C struct{A} 三个类型中A和C可以共享方法，B和A不共享方法；
>
> 11、反射（自省）可以获得类型的字段和方法信息，和Python/Java/CSharp类似，没有深入研究；
>
> 12、defer 在方法返回时调用，如果方法中有多个go 方法，会在每个go 方法调用完后被执行；
>
> 13、类型转换，基本类型(string,bytes,int,float）之间的转换通过内置方法实现
>
> struct通过 struct名称(变量名称)，也可以通过reflect及switch实现(接口变量名.(type))；
>
> 14、change/select/chan 类似与Unix中的管道概念，支持读、写操作；
>
> 15、对于复杂类型的格式化输出可以用 %#v ，这个格式化出来的信息比较全面；
