---
title: HTTP-Client-Transport
date: '2014-07-04'
description:
categories:

tags:golang

---

>

*RoundTripper接口中包含RoundTrip方法，Transport结构中同样包含RoundTrip方法*

*这样一来，接口声明的对象可以接受所有包含RoundTrip方法的接口 ... 仅开放共有方法 ...*

>

	// 为什么[Client]结构定义了[RoundTripper]却使用[&http.Transport]赋值
	http.Client{
		Transport: &http.Transport{
	...
	 
	// Client 结构
	type Client struct {
		// Transport specifies the mechanism by which individual
		// HTTP requests are made.
		// If nil, DefaultTransport is used.
		Transport RoundTripper
	...

	// type Transport
	//   func (t *Transport) CancelRequest(req *Request)
	//   func (t *Transport) CloseIdleConnections()
	//   func (t *Transport) RegisterProtocol(scheme string, rt RoundTripper)
	//   func (t *Transport) RoundTrip(req *Request) (*Response, error)
	// Transport 结构
	type Transport struct {
	...
	 
	// RoundTripper 竟然是个接口
	type RoundTripper interface {
		// RoundTrip executes a single HTTP transaction, returning
		// the Response for the request req.  RoundTrip should not
		// attempt to interpret the response.  In particular,
		// RoundTrip must return err == nil if it obtained a response,
		// regardless of the response's HTTP status code.  A non-nil
		// err should be reserved for failure to obtain a response.
		// Similarly, RoundTrip should not attempt to handle
		// higher-level protocol details such as redirects,
		// authentication, or cookies.
		//
		// RoundTrip should not modify the request, except for
		// consuming and closing the Body. The request's URL and
		// Header fields are guaranteed to be initialized.
		RoundTrip(*Request) (*Response, error)
	}
	...
	 
	// 接口中实现的方法是属于[Transport]的
	func (t *Transport) RoundTrip(req *Request) (resp *Response, err error)
	...
	 
	 
所以....

>

	package main

	import (
		"log"
	)

	// 比如说你定义了一个接口
	type Start interface {
		// 包含了一个Run方法
		Run()
	}

	// 不同结构包含相同方法
	// A
	type A struct {
	}

	func (this *A) Run() {
		log.Println("A")
	}

	// B
	type B struct {
	}

	func (this *B) Run() {
		log.Println("B")
	}

	// C
	type C struct {
	}

	func (this *C) Run() {
		log.Println("C")
	}

	// 使用 ...
	func MainStart(this Start) {
		this.Run()
	}

	func main() {
		MainStart(new(A))
		MainStart(new(B))
		MainStart(new(C))
	}

>

