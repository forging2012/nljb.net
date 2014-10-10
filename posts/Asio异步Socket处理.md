---
title: Asio异步Socket处理
date: '2014-10-10'
description:
categories:

tags:c++

---
.h

	#ifndef XPLAYEVENT_H
	#define XPLAYEVENT_H

	#include <boost/asio.hpp>
	#include <boost/asio/ip/address.hpp>
	#include <boost/bind.hpp>
	#include <boost/shared_ptr.hpp>

	using namespace std;
	using namespace boost::asio;
	using namespace boost;

	class XplayEvent
	{
	public:
	    XplayEvent(io_service& io):ios(io),
		acceptor(ios, ip::tcp::endpoint(ip::tcp::v4(), 6688))
	    {
		start();
	    }
	private:
	    io_service& ios;
	    ip::tcp::acceptor acceptor;
	    typedef boost::shared_ptr<ip::tcp::socket> sock_pt;
	public:
	    void start();
	    void handler(const system::error_code&, sock_pt);
	    void write(const system::error_code&);
	};

	#endif // XPLAYEVENT_H

---
.cpp

	#include "include/XplayEvent.h"
	#include <glog/logging.h>

	void XplayEvent::start()
	{
	    sock_pt sock(new ip::tcp::socket(ios));
	    acceptor.async_accept(*sock,
				  bind(&XplayEvent::handler, this, boost::asio::placeholders::error, sock));
	}

	void XplayEvent::handler(const system::error_code& ec, sock_pt sock)
	{
	    if(ec)
		return;
	    LOG(INFO) << "client:" << sock->remote_endpoint().address() << endl;
	    sock->async_write_some(buffer("hello asio\n"),
				   bind(&XplayEvent::write, this, boost::asio::placeholders::error));
	    sock->async_write_some(buffer("hello asio\n"),
				   bind(&XplayEvent::write, this, boost::asio::placeholders::error));
	    start();
	}

	void XplayEvent::write(const system::error_code&)
	{
	    LOG(INFO) << "send mes complete." << endl;
	}

