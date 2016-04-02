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

#### 互传文件

>

	$ docker cp a77a72ac178c:/var/www/html /var/www/
	// 将容器的文件拷贝到宿主机
	
	$ docker inspect -f '{{.Id}}' a77a72ac178c
	// 将宿主机的文件拷贝到容器内 ...
	// 输出: a77a72ac178c1e35708d2af446197c10239b0b1bd8932104578e334b83eb93a2
	
	// 下一步 ...
	$ cp /root/golang /var/lib/docker/aufs/mnt <接下行>
	a77a72ac178c1e35708d2af446197c10239b0b1bd8932104578e334b83eb93a2/root/
	// 回到容器内即可看到拷贝过去的文件

>

#### 深入容器

>

	$ docker inspect MyName
	// 来获取更多容器的信息
	
	$ docker inspect MyNmae --format='{{ .State.Running }}' MyNmae
	// 通过 --format 来查询容器的运行状态 ...
	
	$ docker inspect MyNmae --format='{{ .NetworkSettings.IPAddress }}' MyNmae
	// 通过 --format 来查询容器的 IP 地址
	
>

---

>

#### 镜像相关命令

>

	$ docker login
	// 镜像保存在 Docker Hub 中 ...
	// 这条命了将完成登陆到 Docker Hub 中 ...

>

	$ docker images
	$ docker images ubuntu
	// 列出本地镜像 ...
	// 本地镜像保存在/var/lib/docker目录下
	
	$ docker pull ubuntu 
	$ docker pull ubuntu:11.04
	// 拉取镜像 ...
	// 镜像是保存在 Docker Hub 中的，可以是官方的也可以是三方的
	
	$ docker search ubuntu 
	// 在 Docker Hub 中查找镜像
	
	$ docker rmi ubuntu
	$ docker rmi `docker images -a -q	`
	// 删除镜像
	// 注意：不是容器，是镜像
	
	// 注意：
	// 我们虽然称之为ubuntu操作系统，但实际上它并不是完成的操作系统
	// 它只是一个裁剪版，由官方制作的，安全的裁剪版本 ...

>

#### 构建镜像

>

	* 使用 docker commit 命令
	* 使用 docker build 命令 和 Dockerfile 文件 ...

>

	// docker commit
	
	$ docker run -i -t ubuntu /bin/bash
	// 运行一个镜像, 并且做好修改，....
	// 然后退出镜像
	
	$ docker ps -a 
	$ docker ps -l -q
	// 可以看一下刚刚修改的容器ID
	
	$ docker commit f7cbdac22a02 nljb/apache2:webserver
	// 讲容器生成一个新的镜像 ...
	// :webserver 为该镜像增加一个标签 ...
	
	$ docker commi -m="A new custom image" --author="nljb" f7cbdac22a02 nljb/webserver
	// -m 指定镜像的提交信息 ...
	// --author 用来指定镜像的作者信息 ...
	
>

#### 镜像其它

	$ docker run -d -p 80 ubuntu /bin/bash
	* 可以在宿主机器上随机选择49153～65535的端口号来映射到容器的80端口
	* 可以在宿主机器上指定一个具体的端口号来映射到容器的80端口上 ...

	$ docker run -d -p 8080(宿主):80(容器) ubuntu /bin/bash
	// 将“容器的80端口”绑定到“宿主机的8080端口”上 ...
	
	$ docker run -d -p 127.0.0.1:80(宿主):80(容器) ubuntu /bin/bash
	// 将容器的80端口绑定到宿主机的127.0.0.1这个IP的80端口上 ...

	$ docker run -d -p 127.0.0.1(宿主)::80(容器) ubuntu /bin/bash
	// 只指定宿主机的IP地址，没有端口号，则 docker 会随机指派端口号 ...
	
	$ docker run -d -P ubuntu /bin/bash
	// -P 该命令会将容器80端口对本地宿主机公开，并且绑定到宿主机的一个随机端口上
	// 有了这个端口号，就可以在宿主机中通过127.0.0.1连接到运行的容器查看Web了

	$ docker ps -l 
	// 可以看到包含端口映射的相关信息
	
	$ docker port f7cbdac22a02 80
	// 通过容器ID查看映射到宿主机80端口的容器端口号
	
>

#### 工作目录

>

	$ docker run -it -w /var/log ubuntu pwd
	// 该命令会更改容器内的工作目录 ...
	// 输出：/var/log
	
>

#### 环境变量

>
	
	$ docker run -it -e "GOPATH=/root/src" ubuntu env
	// -e 标志来传递环境变量, 这些变量只在运行时有效 ...
	// 输出：GOPATH=/root/src

>

#### 推送镜像 Docker Hub

>

	$ docker push youruser/yourimage
	// 将镜像推送到 Docker Hub 仓库

>