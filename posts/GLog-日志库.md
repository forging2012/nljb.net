---
title: GLog-日志库
date: '2014-10-09'
description:
categories:

tags:c++

---

Google Glog 是一个C++语言的应用级日志记录框架，提供了C++风格的流操作和各种助手宏。

代码位于谷歌代码“google-glog”项目

---
安装

	// 下载地址
	https://code.google.com/p/google-glog/downloads/list

	// 安装
	./configure ; make ; make install

	// Lib
	LIBS += -lglog

---
示例

	#include <iostream>
	#include <glog/logging.h>

	using namespace std;

	// 日志支持类型
	// INFO = GLOG_INFO
	// WARNING = GLOG_WARNING,
	// ERROR = GLOG_ERROR
	// FATAL = GLOG_FATAL;

	int main(int argc,char *argv[])
	{
	    // 输出日志到文件
	    // 如果不设置输出文件,则默认输出到/tmp/下
	    google::InitGoogleLogging(argv[0]);

	    // 设置 google::INFO 级别的日志存储路径和文件名前缀
	    // google::SetLogDestination(google::INFO,LOGDIR"/INFO_");
	    // 设置 google::WARNING 级别的日志存储路径和文件名前缀
	    // google::SetLogDestination(google::WARNING,LOGDIR"/WARNING_");
	    // 设置 google::ERROR 级别的日志存储路径和文件名前缀
	    // google::SetLogDestination(google::ERROR,LOGDIR"/ERROR_");
	    // 设置 google::FATAL 级别的日志存储路径和文件名前缀
	    // google::SetLogDestination(google::FATAL,LOGDIR"/FATAL_");

	    // 缓冲日志输出，默认为30秒，此处改为立即输出
	    FLAGS_logbufsecs =0;

	    // 最大日志大小为 100MB
	    FLAGS_max_log_size =100;

	    // 当磁盘被写满时，停止日志输出
	    FLAGS_stop_logging_if_full_disk = true;

	    // 设置文件名扩展，如平台？或其它需要区分的信息
	    google::SetLogFilenameExtension("91_");

	    // 设置级别高于 google::INFO 的日志同时输出到屏幕
	    google::SetStderrLogging(google::INFO);

	    // 设置输出到屏幕的日志显示相应颜色
	    FLAGS_colorlogtostderr=true;

	    // 当条件满足时输出日志
	    LOG_IF(INFO, 20 > 10) << "OK";

	    // 输出信息
	    LOG(INFO)    << "log message..." << endl;
	    LOG(WARNING) << "log message..." << endl;
	    LOG(ERROR)   << "log message..." << endl;
	    LOG(FATAL)   << "log message..." << endl;

	    // 输出结束
	    google::ShutdownGoogleLogging();
	}


