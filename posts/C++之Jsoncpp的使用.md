---
title: C++之Jsoncpp的使用
date: '2014-07-03'
description:
categories:

tags:c++

---

*. 下载jsoncpp源码

	C++要使用JSON来解析数据，一般采用jsoncpp. 
	网站：http://sourceforge.net/projects/jsoncpp/
	我下载的时候最新版本是 jsoncpp-src-0.5.0.tar.gz
	这里有一个拷贝（http://dl.iteye.com/topics/download/4cb5ff91-e210-3e0b-9496-fd31a787a6c7）
	解压到一个目录，然后编译成对应机器可用的lib。然后引入它的json.h头文件就可以使用了。
	 
*. 下载编译jsoncpp的python编译工具并配置环境变量到临时console

	关键是这个lib怎么编译出来，下面说一下步骤：
	编译jsconcpp要使用scons，scons又是一个牛叉的工具，功能和GNU make一样，又比make简单多了。scons是python工具，需要先安装好python。
	下载 scons，解压就可以使用了。
	scons下载地址：http://sourceforge.net/projects/scons/files/scons/2.3.0/ 本文附件中也有一个拷贝
	在要编译jsoncpp的console中导出scons的环境变量临时用一下即可。不必配置到开机启动的环境变量中。
	$export MYSCONS=your_extract_path
	$export MYSCONS_LIB_DIR=$MYSCONS/engine
	 
*. 编译jsoncpp lib

	$ cd jsoncpp-src-0.5.0
	$ python $MYSCONS/script/scons platform=linux-gcc
	一切正常的话，可以在 jsoncpp-src-0.5.0/libs/linux-gcc-x.x 下看到一个动态文件库（so）和一个静态文件库（a）。
 
*. 将jsoncpp库拷贝到系统库下方便使用。

	$ cd jsoncpp-src-0.5.0/libs/linux-gcc-4.7
	$ mv libjson_linux-gcc-4.7_libmt.so libjson.so
	$sudo cp libjson.so /usr/lib

*. 代码演示

	#include <cassert>
	#include "json/json.h"
	using namespace std;
	   
	int main()
	{
	    string json_str = "{\"name\":\"zzg\",\"age\":100}";
	   
	    Json::Reader reader;
	    Json::Value root;
	    if (!reader.parse(json_str, root, false))
	    {
		cout<<"parse error"<<endl;
		return -1;
	    }
	   
	    cout << "test read:" << endl;
	    std::string name = root["name"].asString();
	    int age = root["age"].asInt();
	   
	    std::cout<<name<<std::endl;
	    std::cout<<age<<std::endl;
	     
	  
	    cout<<"test write:"<<endl;
	    Json::FastWriter writer;
	    json_str = writer.write(root);
	    cout<<json_str<<endl;
	    return 0;
	}
	 
	 
	 
	#include <fstream>
	#include <cassert>
	#include "json/json.h"
	using namespace std;
	   
	int main()
	{
	    ifstream ifs;
	    ifs.open("testjson.json");
	    assert(ifs.is_open());
	   
	    Json::Reader reader;
	    Json::Value root;
	    if (!reader.parse(ifs, root, false))
	    {
		return -1;
	    }
	   
	    std::string name = root["name"].asString();
	    int age = root["age"].asInt();
	   
	    std::cout<<name<<std::endl;
	    std::cout<<age<<std::endl;
	   
	    return 0;
	}
	 
	 
	 
	 
	    // 原始
	    string json_str = "{\"name\":\"home\"}";
	 
	    // 解析(Json->Value)
	    Json::Reader reader;
	    Json::Value root;
	    if (!reader.parse(json_str, root, false)) {
		exit(1);
	    }
	 
	    // 读取
	    string name = root["name"].asString();
	    cout << name << std::endl;
	 
	    // 写入(Value->Json)
	    Json::FastWriter writer;
	    json_str = writer.write(root);
	    cout << json_str << endl;


