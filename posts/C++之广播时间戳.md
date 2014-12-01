---
title: C++之广播时间戳
date: '2014-12-01'
description:
categories:

tags:c++

---

	// Broadcast.h
	#ifndef BROADCAST_H
	#define BROADCAST_H

	class Broadcast
	{
	public:
	    Broadcast();
	public:
	    static void run();
	    void start();
	};

	#endif // BROADCAST_H
		
---

	// Broadcast.cpp
	#include "include/Broadcast.h"
	#include "include/Context.h"
	#include "include/Utils.h"
	#include <boost/thread.hpp>
	#include <boost/asio.hpp>
	#include <boost/lexical_cast.hpp>
	#include <glog/logging.h>
	#include <boost/format.hpp>

	using namespace std;
	using namespace boost;
	using namespace boost::asio;
	using boost::asio::ip::udp;
	using boost::asio::ip::address;

	Broadcast::Broadcast(){}

	void Broadcast::run()
	{
	    Broadcast broadcast;
	    broadcast.start();
	}

	void Broadcast::start()
	{
	    try
	    {
		io_service ios;
		boost::system::error_code error;
		int port = Context::GetContext()->config->GetInt("config.broadcast_port", 55601);
		udp::socket sock(ios, udp::endpoint(udp::v4(), port));
		boost::asio::socket_base::broadcast option(true);
		sock.set_option(option);
		udp::endpoint destination(address::from_string("255.255.255.255"), port);
		while(true)
		{
		    uint64_t nano = Utils::GetTimeNS();
		    string ns = lexical_cast<string>(nano);
		    sock.send_to(buffer(ns, ns.size()), destination, 0, error);
		    if(error)
		    {
			LOG(ERROR) << format("Broadcast:: %s") % boost::system::system_error(error).what() << endl;
		    }
		    sleep(10);
		}
	    }
	    catch (std::exception& e)
	    {
		LOG(ERROR) << format("Broadcast:: %s") % e.what() << endl;
		abort();
	    }
	}



