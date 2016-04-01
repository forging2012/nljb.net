---
title: Docker介绍与使用
date: '2016-04-01'
description:
categories:

tags:docker

---

>

### Docker 介绍

>

***docker容器可以理解为在沙盒中运行的进程。这个沙盒包含了该进程运行所必须的资源，包括文件系统、系统类库、shell 环境等等。***

>

***但这个沙盒默认是不会运行任何程序的。你需要在沙盒中运行一个进程来启动某一个容器。这个进程是该容器的唯一进程，所以当该进程结束的时候，容器也会完全的停止。***

>


* Docker是一种Linux容器管理引擎
* Docker(https://www.docker.com)
* Github(https://github.com/docker/docker)
* Go语言编写
* 适用于Linux平台（仅适用）
* Docker是一种实现打包、输送、运行任意应用的容器解决方案

>

***Docker提供隔离的运行环境***

* 文件系统隔离
* 网络隔离
* 进程号隔离
* 进程间通信隔离

***Docker容器受到的资源限制***

* CPU计算资源
* 内存资源
* 磁盘I/O资源等

>

---

>

### Docker 体验

>

* Docker Registry 提供进行下载
* Docker Hub 是 Docker 官方支持的镜像仓库

>

* Docker Hub Mirror
* 缓存 Docker Hub 的镜像
* 加速 Docker Hub 镜像的获取
* 推荐使用DaoCloud加速器
* https://dashboard.daocloud.io/mirror

>

***配置Docker加速器***

* /etc/default/docker (vi)
* DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4"
* 在后面添加 --registry-mirror 阿里云 ...
* 重启 service docker restart
* 下载镜像 docker pull ubuntu:14.04

>

***运行Docker容器***

* 系统容器：Ubuntu、CentOS、Debian等操作系统
* 应用容器：Ghsot应用、2048应用容器，多为（Web)
* 存储容器：MySQL、MongoDB、Redis容器

>

* 查看本地镜像 docker images
* 运行本地镜像 docker run -it ubuntu:14.04 /bin/bash
* 后台运行镜像 docker run -d -P ..

>

***保存Docker容器***

* 运行容器并进行修改 docker run it centos /bin/bash
* 找到修改的容器 docker ps -l (-a) 
* 保存容器的修改 docker commit 698 centos-change 
* 容器ID 698, 镜像名称：centos-change 

>

---

>

# Docker 删除 none 镜像

>

	// 删除 none 镜像
	docker rmi $(docker images | awk '/^<none>/ { print $3 }')
	// 删除失败, 需先停止
	docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
	docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
	// ... 通过 ID 删除 ...
	docker images|grep none|awk '{print $3 }'|xargs docker rmi

>

	// 查看容器的root用户密码
	docker logs <容器名orID> 2>&1 | grep '^User: ' | tail -n1
	
	因为docker容器启动时的root用户的密码是随机分配的。
	通过这种方式就可以得到redmine容器的root用户的密码了。
	
	// 查看容器日志
	docker logs -f <容器名orID>
	
	// 查看正在运行的容器
	docker ps
	
	// 为查看所有的容器，包括已经停止的
	docker ps -a 
	
	// 删除所有容器
	docker rm $(docker ps -a -q)
	
	// 删除单个容器
	docker rm <容器名orID>
	
	// 停止、启动、杀死一个容器
	docker stop <容器名orID>
	docker start <容器名orID>
	docker kill <容器名orID>
	
	// 查看所有镜像
	docker images
	
	// 删除所有镜像
	docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)
	
	// 运行一个新容器，同时为它命名、端口映射、文件夹映射。以redmine镜像为例
	docker run --name redmine -p 9003:80 -p 9023:22 -d -v <接下行>
	/var/redmine/files:/redmine/files -v <接下行>
	/var/redmine/mysql:/var/lib/mysql sameersbn/redmine
	
	// 一个容器连接到另一个容器
	docker run -i -t --name sonar -d -link <接下行>
	mmysql:db tpires/sonar-server sonar
	
	// 容器连接到mmysql容器，并将mmysql容器重命名为db。
	// 这样，sonar容器就可以使用db的相关的环境变量了。
	
	
	// 拉取镜像
	docker pull <镜像名:tag>
	
	// 如
	docker pull sameersbn/redmine:latest
	
	// 当需要把一台机器上的镜像迁移到另一台机器的时候，需要保存镜像与加载镜像。
	
	// 机器 A
	docker save busybox-1 > /home/save.tar
	
	// 使用scp将save.tar拷到机器B上，然后：
	docker load < /home/save.tar
	
	// 构建自己的镜像
	docker build -t <镜像名> <Dockerfile路径>
	
	// 如Dockerfile在当前路径：
	docker build -t xx/gitlab .
	
	// 重新查看container的stdout
	# 启动top命令，后台运行
	$ ID=$(sudo docker run -d ubuntu /usr/bin/top -b)
	# 获取正在running的container的输出
	$ sudo docker attach $ID
	$ sudo docker stop $ID
	
	// 后台运行(-d)、并暴露端口(-p)
	docker run -d -p 127.0.0.1:33301:22 centos6-ssh
	
	// 从container中拷贝文件出来
	sudo docker cp 7bb0e258aefe:/etc/debian_version .

>










