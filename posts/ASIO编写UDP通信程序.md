---
title: ASIO编写UDP通信程序
date: '2014-10-09'
description:
categories:

tags:c++

---

转

ASIO的TCP协议通过boost::asio::ip名空间下的tcp类进行通信

举一返三:ASIO的UDP协议通过boost::asio::ip名空间下的udp类进行通信。

我们知道UDP是基于数据报模式的，所以事先不需要建立连接。

就象寄信一样，要寄给谁只要写上地址往门口的邮箱一丢，其它的事各级邮局包办；

要收信用只要看看自家信箱里有没有信件就行（或问门口传达室老大爷）。

在ASIO里，就是udp::socket的send_to和receive_from方法（异步版本是async_send_to和asnync_receive_from）。

下面的示例代码是从ASIO官方文档里拿来的（实在想不出更好的例子了:-P）

---
服务器端:
 
	//
	// server.cpp
	// ~~~~~~~~~~
	//
	// Copyright (c) 2003-2008 Christopher M. Kohlhoff
	// (chris at kohlhoff dot com)
	//
	// Distributed under the Boost Software License, Version 1.0.
	// (See accompanying
	// file LICENSE_1_0.txt or
	// copy at http://www.boost.org/LICENSE_1_0.txt)
	//
	 
	#include <ctime> www.2cto.com
	#include <iostream>
	#include <string>
	#include <boost/array.hpp>
	#include <boost/asio.hpp>
	 
	using boost::asio::ip::udp;
	 
	std::string make_daytime_string()
	{
	  using namespace std; // For time_t, time and ctime;
	  time_t now = time(0);
	  return ctime(&now);
	}
	 
	int main()
	{
	  try
	   {
	     boost::asio::io_service io_service;
	    // 在本机13端口建立一个socket
	     udp::socket socket(io_service, udp::endpoint(udp::v4(), 13));
	 
	    for (;;)
	     {
	       boost::array<char, 1> recv_buf;
	       udp::endpoint remote_endpoint;
	       boost::system::error_code error;
	      // 接收一个字符，这样就得到了远程端点(remote_endpoint)
	       socket.receive_from(boost::asio::buffer(recv_buf),
		   remote_endpoint, 0, error);
	 
	      if (error && error != boost::asio::error::message_size)
		throw boost::system::system_error(error);
	 
	       std::string message = make_daytime_string();
	      // 向远程端点发送字符串message(当前时间)   
	       boost::system::error_code ignored_error;
	       socket.send_to(boost::asio::buffer(message),
		   remote_endpoint, 0, ignored_error);
	     }
	   }
	  catch (std::exception& e)
	   {
	     std::cerr << e.what() << std::endl;
	   }
	 
	  return 0;
	}
	 

--- 
客户端:
 
 
	//
	// client.cpp
	// ~~~~~~~~~~
	//
	// Copyright (c) 2003-2008 Christopher M. Kohlhoff
	// (chris at kohlhoff dot com)
	//
	// Distributed under the Boost Software License, Version 1.0.
	// (See accompanying file LICENSE_1_0.txt or
	//   copy at http://www.boost.org/LICENSE_1_0.txt)
	//
	 
	#include <iostream>
	#include <boost/array.hpp>
	#include <boost/asio.hpp>
	 
	using boost::asio::ip::udp;
	 
	int main(int argc, char* argv[])
	{
	  try
	   {
	    if (argc != 2)
	     {
	       std::cerr << "Usage: client <host>" << std::endl;
	      return 1;
	     }
	 
	     boost::asio::io_service io_service;
	    // 取得命令行参数对应的服务器端点
	     udp::resolver resolver(io_service);
	     udp::resolver::query query(udp::v4(), argv[1], "daytime");
	     udp::endpoint receiver_endpoint = *resolver.resolve(query);
	 
	     udp::socket socket(io_service);
	     socket.open(udp::v4());
	    // 发送一个字节给服务器，让服务器知道我们的地址
	     boost::array<char, 1> send_buf   = { 0 };
	     socket.send_to(boost::asio::buffer(send_buf), receiver_endpoint);
	    // 接收服务器发来的数据
	     boost::array<char, 128> recv_buf;
	     udp::endpoint sender_endpoint;
	    size_t len = socket.receive_from(
		 boost::asio::buffer(recv_buf), sender_endpoint);
	 
	     std::cout.write(recv_buf.data(), len);
	   }
	  catch (std::exception& e)
	   {
	     std::cerr << e.what() << std::endl;
	   }
	 
	  return 0;
	}

