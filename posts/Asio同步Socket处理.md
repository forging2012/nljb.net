---
title: Asio同步Socket处理
date: '2014-10-10'
description:
categories:

tags:c++

---
关于保存Socket连接

	// 由于栈的原因，连接被释放了...
	ip::tcp::socket sock(ios);
	// 改为堆的方式，连接被保留了...
	ip::tcp::socket * sock = new ip::tcp::socket(ios);

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
	    XplayEvent();
	public:
	    void start();
	};

	#endif // XPLAYEVENT_H


---
.cpp

	#include "include/XplayEvent.h"
	#include <glog/logging.h>

	XplayEvent::XplayEvent()
	{

	}

	void XplayEvent::start()
	{
	    try
	    {
		io_service ios;
		ip::tcp::acceptor acceptor(ios,
					   ip::tcp::endpoint(ip::tcp::v4(), 6688));
		LOG(INFO) << "XplayEvent::" << acceptor.local_endpoint().address() << endl;
		while(true)
		{
		    ip::tcp::socket sock(ios);
		    acceptor.accept(sock);
		    LOG(INFO) << "XplayEvent::" << "xplay ->" << sock.remote_endpoint().address() << endl;
		    sock.write_some(buffer("hello asio\n"));
		}
	    }
	    catch (std::exception& e)
	    {
		LOG(ERROR) << e.what() << endl;
	    }
	}


