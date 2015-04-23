---
title: socket.cpp-监听解码
date: '2014-07-03'
description:
categories:

tags:system

---

	#include <iostream>
	#include <sys/socket.h>
	#include <stdio.h>
	#include <stdlib.h>
	#include <netinet/in.h>
	#include <string.h>
	#include <unistd.h>
	#include <json/json.h>
	 
	using namespace std;
	 
	// 常量
	#define SERVPORT 4444
	#define BACKLOG 10
	#define MAXSIZE 1024
	 
	int main() {
	 
	    // 声明
	    int sockfd, client_fd;
	    struct sockaddr_in my_addr;
	    struct sockaddr_in remote_addr;
	 
	    // 创建
	    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1) {
		perror("socket create failed!");
		exit(1);
	    }
	 
	    // 绑定端口地址
	    my_addr.sin_family = AF_INET;
	    my_addr.sin_port = htons(SERVPORT);
	    my_addr.sin_addr.s_addr = INADDR_ANY;
	    bzero(&(my_addr.sin_zero), 8);
	 
	    // 捆绑
	    if (bind(sockfd, (struct sockaddr*) &my_addr, sizeof(struct sockaddr))
		    == -1) {
		perror("bind error!");
		exit(1);
	    }
	 
	    // 监听端口
	    if (listen(sockfd, BACKLOG) == -1) {
		perror("listen error");
		exit(1);
	    }
	 
	    // 接受连接
	    int sin_size = sizeof(struct sockaddr_in);
	    if ((client_fd = accept(sockfd, (struct sockaddr *) &remote_addr,
		    (socklen_t *) &sin_size)) == -1) {
		perror("accept error!");
		exit(1);
	    }
	 
	    // 变量
	    string json_str;
	    char buf[MAXSIZE];
	 
	    // 处理
	    while (1) {
	 
		// 归零
		bzero(buf, MAXSIZE);
	 
		// 接受client发送的请示信息
		if (recv(client_fd, buf, MAXSIZE, 0) == 0)
		    continue;
	 
		buf[strlen(buf) - 1] = ' ';
		buf[strlen(buf) - 2] = '\0';
	 
		if (strcmp(buf, "QUIT") == 0) {
		    printf("%s\n", "bye!");
		    close(sockfd);
		    exit(0);
		}
	 
		// (Char[]->String)
		json_str.insert(0, buf);
	 
		// 解析(Json->Value)
		Json::Reader reader;
		Json::Value root;
		if (!reader.parse(json_str, root, false)) {
		    cout << "parse error" << endl;
		    exit(1);
		}
	 
		// 获取
		cout << "Name:" << root["name"].asString() << endl;
	 
		// 写入(Value->Json)
		Json::FastWriter writer;
		json_str = writer.write(root);
		cout << json_str << endl;
	 
	    }
	 
	    // 返回
	    return 0;
	}

