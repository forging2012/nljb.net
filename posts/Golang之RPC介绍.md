---
title: Golang之RPC介绍
date: '2015-05-26'
description:
categories:

tags:golang

---

>

### RPC

>

标准库的RPC

>

RPC是远程调用的简称, 简单的说就是要像调用本地函数一样调用服务器的函数.

>

Go语言的标准库已经提供了RPC框架和不同的RPC实现.

>

---

>

### 下面是一个服务器的例子

>

	type Echo int

	func (t *Echo) Hi(args string, reply *string) error {
	    *reply = "echo:" + args
	    return nil
	}

	func main() {
	    rpc.Register(new(Echo))
	    rpc.HandleHTTP()
	    l, e := net.Listen("tcp", ":1234")
	    if e != nil {
		log.Fatal("listen error:", e)
	    }
	    http.Serve(l, nil)
	}

	其中 rpc.Register 用于注册RPC服务, 默认的名字是对象的类型名字(这里是Echo).

	如果需要指定特殊的名字, 可以用 rpc.RegisterName 进行注册.

>

	被注册对象的类型所有满足以下规则的方法会被导出到RPC服务接口:

	func (t *T) MethodName(argType T1, replyType *T2) error

	// 也就是一个方法带有两个参数(类或类型)，第一个是接收参数，第二个返回参数（类或类型的指针）+ error

>

	被注册对应至少要有一个方法满足这个特征, 否则可能会注册失败.
	
	然后 rpc.HandleHTTP 用于指定 RPC 的传输协议, 这里是采用 http 协议作为RPC调用的载体.

	用户也可以用rpc.ServeConn接口, 定制自己的传输协议.

>

---

>

### 客户端可以这样调用Echo.Hi接口:

>

	func main() {
	    client, err := rpc.DialHTTP("tcp", "127.0.0.1:1234")
	    if err != nil {
		log.Fatal("dialing:", err)
	    }

	    var args = "hello rpc"
	    var reply string
	    err = client.Call("Echo.Hi", args, &reply)
	    if err != nil {
		log.Fatal("arith error:", err)
	    }
	    fmt.Printf("Arith: %d*%d=%d\n", args.A, args.B, reply)
	}

>

	客户端先用rpc.DialHTTP和RPC服务器进行一个链接(协议必须匹配).

	然后通过返回的client对象进行远程函数调用. 

	函数的名字是由client.Call 第一个参数指定(是一个字符串).

	基于HTTP的RPC调用一般是在调试时使用, 默认可以通过浏览"127.0.0.1:1234/debug/rpc"页面查看RPC的统计信息.

>

---

>

### 基于 JSON 的 RPC 调用

>

	Go的标准库还提供了一个"net/rpc/jsonrpc"包, 用于提供基于JSON编码的RPC支持.

	服务器部分只需要用rpc.ServeCodec指定json编码协议就可以了:

	func main() {
	    lis, err := net.Listen("tcp", ":1234")
	    if err != nil {
		return err
	    }
	    defer lis.Close()

	    srv := rpc.NewServer()
	    if err := srv.RegisterName("Echo", new(Echo)); err != nil {
		return err
	    }

	    for {
		conn, err := lis.Accept()
		if err != nil {
		    log.Fatalf("lis.Accept(): %v\n", err)
		}
		go srv.ServeCodec(jsonrpc.NewServerCodec(conn))
	    }
	}

>

	客户端部分值需要用 jsonrpc.Dial 代替 rpc.Dial 就可以了:

	func main() {
	    client, err := jsonrpc.DialHTTP("tcp", "127.0.0.1:1234")
	    if err != nil {
		log.Fatal("dialing:", err)
	    }
	    ...
	}

>

Go语言的RPC客户端是一个使用简单, 而且功能强大的RPC库.

基于标准的RPC库我们可以方便的定制自己的RPC实现(传输协议和串行化协议都可以定制).

以后有时间，我会分析一下RPC这个库，来了解一下具体的实现，而且内部使用了很多反射
