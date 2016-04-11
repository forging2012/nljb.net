---
title: HTTP-Client-Transport
date: '2014-07-04'
description:
categories:

tags:golang

---

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

