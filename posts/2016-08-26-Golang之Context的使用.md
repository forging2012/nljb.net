---
title: Golang之Context的使用
date: '2016-08-26'
description:
categories:

tags:golang

---


#### 简介

>

    在golang中的创建一个新的线程并不会返回像c语言类似的pid
    所有我们不能从外部杀死某个线程，所有我就得让它自己结束
    之前我们用channel＋select的方式，来解决这个问题
    但是有些场景实现起来比较麻烦，例如由一个请求衍生出多个线程
    并且之间需要满足一定的约束关系，以实现一些诸如：
    有效期，中止线程树，传递请求全局变量之类的功能。
    于是google 就为我们提供一个解决方案，开源了context包。
    使用context包来实现上下文功能 .....
    约定：需要在你的方法的传入参数的第一个参数是context.Context的变量。
    
>

---

#### 源码剖析

>

***context.Context 接口***

* context包里的方法是线程安全的，可以被多个线程使用    
* 当Context被canceled或是timeout, Done返回一个被closed 的channel   
* 在Done的channel被closed后, Err代表被关闭的原因   
* 如果存在, Deadline 返回Context将要关闭的时间  
* 如果存在，Value 返回与 key 相关了的值，不存在返回 nil  

>

    // context 包的核心
    type Context interface {               
        Done() <-chan struct{}      
        Err() error 
        Deadline() (deadline time.Time, ok bool)
        Value(key interface{}) interface{}
    }
    
>

***我们不需要手动实现这个接口，context 包已经给我们提供了两个***

>

    一个是 Background()，一个是 TODO()
    这两个函数都会返回一个Context的实例
    只是返回的这两个实例都是空 Context。

>

	/*
		TODO返回一个非空，空的上下文
		在目前还不清楚要使用的上下文或尚不可用时
	*/
	context.TODO()
	/*
		Background返回一个非空，空的上下文。
		这是没有取消，没有值，并且没有期限。
		它通常用于由主功能，初始化和测试，并作为输入的顶层上下文
	*/
	context.Background()
	
>

#### 主要方法

>

    func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
    func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
    func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
    func WithValue(parent Context, key interface{}, val interface{}) Context
    
>

***WithCancel 对应的是 cancelCtx ,其中，返回一个 cancelCtx ，同时返回一个 CancelFunc，CancelFunc 是 context 包中定义的一个函数类型：type CancelFunc func()。调用这个 CancelFunc 时，关闭对应的c.done，也就是让他的后代goroutine退出***

>

***WithDeadline 和 WithTimeout 对应的是 timerCtx ，WithDeadline 和 WithTimeout 是相似的，WithDeadline 是设置具体的 deadline 时间，到达 deadline 的时候，后代 goroutine 退出，而 WithTimeout 简单粗暴，直接 return WithDeadline(parent, time.Now().Add(timeout))***

>

***WithValue 对应 valueCtx ，WithValue 是在 Context 中设置一个 map，拿到这个 Context 以及它的后代的 goroutine 都可以拿到 map 里的值***

>

#### context的创建

>

***所有的context的父对象，也叫根对象，是一个空的context，它不能被取消，它没有值，从不会被取消，也没有超时时间，它常常作为处理request的顶层context存在，然后通过WithCancel、WithTimeout函数来创建子对象来获得cancel、timeout的能力***

***当顶层的request请求函数结束后，我们就可以cancel掉某个context，从而通知别的routine结束***

***WithValue方法可以把键值对加入context中，让不同的routine获取***

>

### 官方案例 

>

    // 在 handle 环境中使用 
    func handleSearch(w http.ResponseWriter, req *http.Request) {
        // ctx is the Context for this handler. Calling cancel closes the
        // ctx.Done channel, which is the cancellation signal for requests
        // started by this handler.
        var (
            ctx    context.Context
            cancel context.CancelFunc
        )
        // 获取参数 ...
        timeout, err := time.ParseDuration(req.FormValue("timeout"))
        if err == nil {
            // The request has a timeout, so create a context that is
            // canceled automatically when the timeout expires.
            // 获取成功, 则按照参数设置超时时间
            ctx, cancel = context.WithTimeout(context.Background(), timeout)
        } else {
            // 获取失败, 则在该函数结束时结束 ...
            ctx, cancel = context.WithCancel(context.Background())
        }
        // ----------------
        // 这样随着cancel的执行,所有的线程都随之结束了 ...
        go A(ctx) +1
        go B(ctx) +2
        go C(ctx) +3
        // ----------------
        defer cancel() // Cancel ctx as soon as handleSearch returns.
    }
    
    // 监听 ctx.Done() 结束 ...
    func A(ctx context.Context) int {
        // ... TODO
    	select {
    	case <-ctx.Done():
    		return -1
	default:
		// 没有结束 ... 执行 ...
    	}
    }
    
>

#### ctxhttp.go

>

    package ctxhttp // import "golang.org/x/net/context/ctxhttp"
    
    import (
    	"io"
    	"net/http"
    	"net/url"
    	"strings"
    
    	"golang.org/x/net/context"
    )
    
    // Do sends an HTTP request with the provided http.Client and returns
    // an HTTP response.
    //
    // If the client is nil, http.DefaultClient is used.
    //
    // The provided ctx must be non-nil. If it is canceled or times out,
    // ctx.Err() will be returned.
    func Do(ctx context.Context, client *http.Client, req *http.Request) (*http.Response, error) {
    	if client == nil {
    		client = http.DefaultClient
    	}
    	resp, err := client.Do(req.WithContext(ctx))
    	// If we got an error, and the context has been canceled,
    	// the context's error is probably more useful.
    	if err != nil {
    		select {
    		case <-ctx.Done():
    			err = ctx.Err()
    		default:
    		}
    	}
    	return resp, err
    }
    
    // Get issues a GET request via the Do function.
    func Get(ctx context.Context, client *http.Client, url string) (*http.Response, error) {
    	req, err := http.NewRequest("GET", url, nil)
    	if err != nil {
    		return nil, err
    	}
    	return Do(ctx, client, req)
    }
    
    // Head issues a HEAD request via the Do function.
    func Head(ctx context.Context, client *http.Client, url string) (*http.Response, error) {
    	req, err := http.NewRequest("HEAD", url, nil)
    	if err != nil {
    		return nil, err
    	}
    	return Do(ctx, client, req)
    }
    
    // Post issues a POST request via the Do function.
    func Post(ctx context.Context, client *http.Client, url string, bodyType string, body io.Reader) (*http.Response, error) {
    	req, err := http.NewRequest("POST", url, body)
    	if err != nil {
    		return nil, err
    	}
    	req.Header.Set("Content-Type", bodyType)
    	return Do(ctx, client, req)
    }
    
    // PostForm issues a POST request via the Do function.
    func PostForm(ctx context.Context, client *http.Client, url string, data url.Values) (*http.Response, error) {
    	return Post(ctx, client, url, "application/x-www-form-urlencoded", strings.NewReader(data.Encode()))
    }

>


#### 使用示例

>

    package main
    
    import (
    	"fmt"
    	"time"
    
    	"golang.org/x/net/context"
    )
    
    func Cdd(ctx context.Context) int {
    	fmt.Println(ctx.Value("NLJB"))
    	select {
    	// 结束时候做点什么 ...
    	case <-ctx.Done():
    		return -3
	default:
		// 没有结束 ... 执行 ...
    	}
    }
    
    func Bdd(ctx context.Context) int {
    	fmt.Println(ctx.Value("HELLO"))
    	fmt.Println(ctx.Value("WROLD"))
    	ctx = context.WithValue(ctx, "NLJB", "NULIJIABEI")
    	go fmt.Println(Cdd(ctx))
    	select {
    	// 结束时候做点什么 ...
    	case <-ctx.Done():
    		return -2
	default:
		// 没有结束 ... 执行 ...
    	}
    }
    
    func Add(ctx context.Context) int {
    	ctx = context.WithValue(ctx, "HELLO", "WROLD")
    	ctx = context.WithValue(ctx, "WROLD", "HELLO")
    	go fmt.Println(Bdd(ctx))
    	select {
    	// 结束时候做点什么 ...
    	case <-ctx.Done():
    		return -1
	default:
		// 没有结束 ... 执行 ...
    	}
    }
    
    func main() {
    
    	// 自动取消(定时取消)
    	{
    		timeout := 3 * time.Second
    		ctx, _ := context.WithTimeout(context.Background(), timeout)
    		fmt.Println(Add(ctx))
    	}
    	// 手动取消
    	//	{
    	//		ctx, cancel := context.WithCancel(context.Background())
    	//		go func() {
    	//			time.Sleep(2 * time.Second)
    	//			cancel() // 在调用处主动取消
    	//		}()
    	//		fmt.Println(Add(ctx))
    	//	}
    	select {}
    
    }

>

    package main
    
    import (
        "fmt"
        "time"
        "golang.org/x/net/context"
    )
    
    // 模拟一个最小执行时间的阻塞函数
    func inc(a int) int {
        res := a + 1                // 虽然我只做了一次简单的 +1 的运算,
        time.Sleep(1 * time.Second) // 但是由于我的机器指令集中没有这条指令,
        // 所以在我执行了 1000000000 条机器指令, 续了 1s 之后, 我才终于得到结果。B)
        return res
    }
    
    // 向外部提供的阻塞接口
    // 计算 a + b, 注意 a, b 均不能为负
    // 如果计算被中断, 则返回 -1
    func Add(ctx context.Context, a, b int) int {
        res := 0
        for i := 0; i < a; i++ {
            res = inc(res)
            select {
            case <-ctx.Done():
                return -1
            default:
	    	// 没有结束 ... 执行 ...
            }
        }
        for i := 0; i < b; i++ {
            res = inc(res)
            select {
            case <-ctx.Done():
                return -1
            default:
	    	// 没有结束 ... 执行 ...
            }
        }
        return res
    }
    
    func main() {
        {
            // 使用开放的 API 计算 a+b
            a := 1
            b := 2
            timeout := 2 * time.Second
            ctx, _ := context.WithTimeout(context.Background(), timeout)
            res := Add(ctx, 1, 2)
            fmt.Printf("Compute: %d+%d, result: %d\n", a, b, res)
        }
        {
            // 手动取消
            a := 1
            b := 2
            ctx, cancel := context.WithCancel(context.Background())
            go func() {
                time.Sleep(2 * time.Second)
                cancel() // 在调用处主动取消
            }()
            res := Add(ctx, 1, 2)
            fmt.Printf("Compute: %d+%d, result: %d\n", a, b, res)
        }
    }
    
>

***声明：本文转自http://www.01happy.com/golang-context-reading/有修改***

>
