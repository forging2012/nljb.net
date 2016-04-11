---
title: Golang Package Sync
date: '2014-12-10'
description:
categories:

tags:golang

---

	import "sync"

---

	type Locker interface {
	    Lock()
	    Unlock()
	}
	// Locker接口代表一个可以加锁和解锁的对象。

---

	type Once struct {
	    // 包含隐藏或非导出字段
	}
	// Once是只执行一次动作的对象。

	func (o *Once) Do(f func())
        // Do方法当且仅当第一次被调用时才执行函数f。换句话说，给定变量：

	var once Once
        // 如果once.Do(f)被多次调用，只有第一次调用会执行f，即使f每次调用Do 提供的f值不同。

	// 需要给每个要执行仅一次的函数都建立一个Once类型的实例。

	// Do用于必须刚好运行一次的初始化。

	// 因为f是没有参数的，因此可能需要使用闭包来提供给Do方法调用：
	config.once.Do(func() { config.init(filename) })
	// 因为只有f返回后Do方法才会返回，f若引起了Do的调用，会导致死锁。

	var once sync.Once
	onceBody := func() {
	    fmt.Println("Only once")
	}
	done := make(chan bool)
	for i := 0; i < 10; i++ {
	    go func() {
		once.Do(onceBody)
		done <- true
	    }()
	}
	for i := 0; i < 10; i++ {
	    <-done
	}

---

	type Mutex struct {
	    // 包含隐藏或非导出字段
	}
	// Mutex是一个互斥锁，可以创建为其他结构体的字段；
	// 零值为解锁状态。Mutex类型的锁和线程无关，可以由不同的线程加锁和解锁。

	func (m *Mutex) Lock()
	// Lock方法锁住m，如果m已经加锁，则阻塞直到m解锁。

	func (m *Mutex) Unlock()
	// Unlock方法解锁m，如果m未加锁会导致运行时错误。
	// 锁和线程无关，可以由不同的线程加锁和解锁。
		
---

	type RWMutex struct {
    		// 包含隐藏或非导出字段
	}
	// RWMutex是读写互斥锁。
	// 该锁可以被同时多个读取者持有或唯一个写入者持有。
	// RWMutex可以创建为其他结构体的字段；零值为解锁状态。
	// RWMutex类型的锁也和线程无关，可以由不同的线程加读取锁/写入和解读取锁/写入锁。

	func (rw *RWMutex) Lock()
	// Lock方法将rw锁定为写入状态，禁止其他线程读取或者写入。

	func (rw *RWMutex) Unlock()
	// Unlock方法解除rw的写入锁状态，如果m未加写入锁会导致运行时错误。

	func (rw *RWMutex) RLock()
	// RLock方法将rw锁定为读取状态，禁止其他线程写入，但不禁止读取。

	func (rw *RWMutex) RUnlock()
	// Runlock方法解除rw的读取锁状态，如果m未加读取锁会导致运行时错误。

	func (rw *RWMutex) RLocker() Locker
	// Rlocker方法返回一个互斥锁，通过调用rw.Rlock和rw.Runlock实现了Locker接口。	

---

	type WaitGroup struct {
    		// 包含隐藏或非导出字段
	}
	// WaitGroup用于等待一组线程的结束。
	// 父线程调用Add方法来设定应等待的线程的数量。
	// 每个被等待的线程在结束时应调用Done方法。
	// 同时，主线程里可以调用Wait方法阻塞至所有线程结束。

	var wg sync.WaitGroup
	var urls = []string{
	    "http://www.golang.org/",
	    "http://www.google.com/",
	    "http://www.somestupidname.com/",
	}
	for _, url := range urls {
	    // Increment the WaitGroup counter.
	    wg.Add(1)
	    // Launch a goroutine to fetch the URL.
	    go func(url string) {
		// Decrement the counter when the goroutine completes.
		defer wg.Done()
		// Fetch the URL.
		http.Get(url)
	    }(url)
	}
	// Wait for all HTTP fetches to complete.
	wg.Wait()

	func (wg *WaitGroup) Add(delta int)
	// Add方法向内部计数加上delta，delta可以是负数；
	// 如果内部计数器变为0，Wait方法阻塞等待的所有线程都会释放，如果计数器小于0，方法panic。
	// 注意Add加上正数的调用应在Wait之前，否则Wait可能只会等待很少的线程。
	// 一般来说本方法应在创建新的线程或者其他应等待的事件之前调用。

	func (wg *WaitGroup) Done()
	// Done方法减少WaitGroup计数器的值，应在线程的最后执行。

	func (wg *WaitGroup) Wait()
	// Wait方法阻塞直到WaitGroup计数器减为0。

---

	type Cond struct {
	    // 在观测或更改条件时L会冻结
	    L Locker
	    // 包含隐藏或非导出字段
	}
	// Cond实现了一个条件变量，一个线程集合地，供线程等待或者宣布某事件的发生。

	// 每个Cond实例都有一个相关的锁（一般是*Mutex或*RWMutex类型的值）
	// 它必须在改变条件时或者调用Wait方法时保持锁定。
	// Cond可以创建为其他结构体的字段，Cond在开始使用后不能被拷贝。

	func NewCond(l Locker) *Cond
	// 使用锁l创建一个*Cond。

	func (c *Cond) Broadcast()
	// Broadcast唤醒所有等待c的线程。
	// 调用者在调用本方法时，建议（但并非必须）保持c.L的锁定。

	func (c *Cond) Signal()
	// Signal唤醒等待c的一个线程（如果存在）。
	// 调用者在调用本方法时，建议（但并非必须）保持c.L的锁定。

	func (c *Cond) Wait()
	// Wait自行解锁c.L并阻塞当前线程，在之后线程恢复执行时，Wait方法会在返回前锁定c.L。
	// 和其他系统不同，Wait除非被Broadcast或者Signal唤醒，不会主动返回。

	// 因为线程中Wait方法是第一个恢复执行的，而此时c.L未加锁。
	// 调用者不应假设Wait恢复时条件已满足，相反，调用者应在循环中等待：

	c.L.Lock()
	for !condition() {
	    c.Wait()
	}
	... make use of condition ...
	c.L.Unlock()

---

	// 本博客另行介绍
	type Pool struct {
	    // 可选参数New指定一个函数在Get方法可能返回nil时来生成一个值
	    // 该参数不能在调用Get方法时被修改
	    New func() interface{}
	    // 包含隐藏或非导出字段
	}

