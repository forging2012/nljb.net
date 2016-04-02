---
title: The Docker Book
date: '2016-04-02'
description:
categories:

tags:docker

---

>

### The Docker Book

>

	$ docker info
	// 该命令会返回所有容器的镜像的数量
	// Docker 使用的执行驱动和存储驱动
	// 以及 Docker 的基本配置
	
>

#### 运行容器

>

	$ docker run -i -t ubuntu /bin/bash
	// 运行 ubuntu 创建新容器
	// -i 保证容器中STDIN是开启的
	// -t 为创建的容器分配一个伪终端
	// root@f7cbdac22a02:/#
	
	$ docker run -i -t -d ubuntu /bin/bash
	// -d 代表 Docker 容器将在后台运行 ...
	
	$ docker run -d ubuntu /bin/bash 
	// 如果这样运行，则容器会立即停止
	// 因为没有程序在容器中运行 ...
	// -t 也是个程序，所以容器不会停止 ...
	
>

#### 使用容器

>

	root@f7cbdac22a02:/#
	// 该容器为一个完整的 ubuntu 系统
	// 按你的需求使用它即可
	// 一旦推出容器，/bin/bash 命令也就结束了
	// 这时容器也随之停止了运行
	
>

	$ docker ps
	// 只能看到正在运行的容器
	
	$ docker ps -a
	// 可以看到所有的容器，包括已经停止的
	
>

***有三中方式可以指代唯一容器：***

* 短UUID（如：f7cbdac22a02)
* 长UUID（如：f7cbdac22a02f7cbdac22a02f7cbdac22a02f7cbdac22a02）
* 或者名称（如：gray_cat）

>

#### 容器命名

>

	$ docker run --name MyName -i -t ubuntu /bin/bash
	// 通过 --name 为容器指定一个名称 [a-zA-Z0-9_.-]
	
>

#### 重新启动、停止、删除容器

>

	// 注意：docker run 每次运行都会创建一个全新的容器
	
	$ docker start MyName 
	$ docker start f7cbdac22a02
	// 除了容器名称，还可以使用容器ID来指定容器
	
	$ docker restart f7cbdac22a02 
	// 重新启动一个容器 ...
	
	// 重新启动的容器会沿用 docker run 命令时指定的参数来运行
	
	$ docker stop MyName
	// 停止容器的运行 ...
	
	$ docker kill MyName
	// 强制停止容器的运行 ...
	
	$ docker rm MyName
	// 删除一个已经停止的容器
	
	$ docker rm `docker ps -a -q`
	// 删除所有的容器

>

#### 附着到容器上

>
	
	$ docker attach MyName
	// 重新附着到容器上 ...
	// 可能需要按下回车键才能呢个进入该会话
	
	$ docker exec -t -i MyName /bin/bash
	// 可以通过运行一个模拟终端的交互任务来附着容器
	
>

#### 创建守护式容器

>

	$ docker run --name MyName -d ubuntu /bin/sh -c 
		"while echo hello; sleep 1; done"
	// 该循环会一直运行下去，直到容器或其进程停止运行 ...
	
>

#### 容器内容都在干什么 ...

>

	$ docker logs MyName 
	// Docker 会输出最后几条日期项并返回
	
	$ docker logs -f MyName
	// 跟踪守护式容器的日志
	// Ctrl + C 退出跟踪
	
	$ docker logs -ft MyName
	// -t 为每条日志项加上时间戳
	
	$ docker top MyName
	// 查看容器内的进程
	
>

#### 在容器内部运行进程

>
	
	// 注意：
	// 在容器内运行的进程有两种类型，后台任务和交互任务
	// 后台任务在容器内运行且没有交互需求
	// 而交互任务则保持在前台运行 ...

	$ docker exec -d MyName touch /etc/new_config_file
	// 后台任务 ... 在容器内部额外启动新进程（后台任务） ...
	
	$ docker exec -t -i MyName /bin/bash
	// 交互任务 ... 在容器内部启动一个交互任务 ...
	// -t 模拟终端也是个交互程序 ...
	
	
>
	
#### 自动重启容器

>

	$ docker run --restart=always -d ubuntu ...
	// always 无论容器的退出代码是什么，都会自动重启该容器
	// on-failure 只有当容器的退出代码非0时，才会重启容器
	// --restart=on-failure:5，最多重启次数 ...
	
>

#### 深入容器

>

	$ docker inspect MyName
	// 来获取更多容器的信息
	
	$ docker inspect MyNmae --format='{{ .State.Running }}' MyNmae
	// 通过 --format 来查询容器的运行状态 ...
	
	$ docker inspect MyNmae --format='{{ .NetworkSettings.IPAddress }}' MyNmae
	// 通过 --format 来查询容器的 IP 地址
	
	

	