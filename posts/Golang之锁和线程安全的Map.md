---
title: Golang之锁和线程安全的Map
date: '2015-07-29'
description:
categories:

tags:golang

---

>

	Golang的包sync实现了两种类型的锁： sync.Mutex 和 sync.RWMutex。

	通过阅读源代码我们可以知道sync.RWMutex是基于sync.Mutex实现的，其中的只读锁的实现使用类似引用计数的方式。
 
	对于任意 sync.Mutex 或 sync.RWMutex 变量l。
			 如果 n < m ，那么第n次 l.Unlock() 调用在第 m次 l.Lock()调用返回前发生。

>

	// 下面代码来自：
	// https://github.com/astaxie/beego/blob/master/safemap.go
	// 用读写锁实现了安全的map
	package beego
	 
	import (
		"sync"
	)
	 
	type BeeMap struct {
		lock *sync.RWMutex
		bm   map[interface{}]interface{}
	}
	 
	func NewBeeMap() *BeeMap {
		return &BeeMap{
			lock: new(sync.RWMutex),
			bm:   make(map[interface{}]interface{}),
		}
	}
	 
	//Get from maps return the k's value
	func (m *BeeMap) Get(k interface{}) interface{} {
		m.lock.RLock()
		defer m.lock.RUnlock()
		if val, ok := m.bm[k]; ok {
			return val
		}
		return nil
	}
	 
	// Maps the given key and value. Returns false
	// if the key is already in the map and changes nothing.
	func (m *BeeMap) Set(k interface{}, v interface{}) bool {
		m.lock.Lock()
		defer m.lock.Unlock()
		if val, ok := m.bm[k]; !ok {
			m.bm[k] = v
		} else if val != v {
			m.bm[k] = v
		} else {
			return false
		}
		return true
	}
	 
	// Returns true if k is exist in the map.
	func (m *BeeMap) Check(k interface{}) bool {
		m.lock.RLock()
		defer m.lock.RUnlock()
		if _, ok := m.bm[k]; !ok {
			return false
		}
		return true
	}
	 
	func (m *BeeMap) Delete(k interface{}) {
		m.lock.Lock()
		defer m.lock.Unlock()
		delete(m.bm, k)
	}

