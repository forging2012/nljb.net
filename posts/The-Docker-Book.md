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
	
	$ docker version
	// Docker的版本号，API版本号
	// Git commit， Docker客户端和后台进程的Go版本号
	
>

#### 运行容器

>

	$ docker run -i -t nljb.net /bin/bash
	// 运行 nljb.net 创建新容器
	// -i 保证容器中STDIN是开启的
	// -t 为创建的容器分配一个伪终端
	// root@f7cbdac22a02:/#
	
	$ docker run -i -t -d nljb.net /bin/bash
	// -d 代表 Docker 容器将在后台运行 ...
	
	$ docker run -d nljb.net /bin/bash 
	// 如果这样运行，则容器会立即停止
	// 因为没有程序在容器中运行 ...
	// -t 也是个程序，所以容器不会停止 ...
	
	$ docker run -i -t -h nljb nljb.net /bin/bash 
	// -h 标志来为容器设定主机名
	// --hostname 标志来为容器设定主机名
	
>

#### 使用容器

>

	root@f7cbdac22a02:/#
	// 该容器为一个完整的 nljb.net 系统
	// 按你的需求使用它即可
	// 一旦推出容器，/bin/bash 命令也就结束了
	// 这时容器也随之停止了运行
	
>

	$ docker ps
	// 只能看到正在运行的容器
	
	$ docker ps -a
	// 可以看到所有的容器，包括已经停止的
	
>

	有三中方式可以指代唯一容器:

	* 短UUID（如：f7cbdac22a02)
	* 长UUID（如：f7cbdac22a02f7cbdac22a02f7cbdac22a02f7cbdac22a02）
	* 或者名称（如：gray_cat）

>

#### 容器命名

>

	$ docker run --name MyName -i -t nljb.net /bin/bash
	// 通过 --name 为容器指定一个名称 [a-zA-Z0-9_.-]
	
>

#### 重新启动、停止、删除容器

>

	注意：docker run 每次运行都会创建一个全新的容器
	
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

	$ docker run --name MyName -d nljb.net /bin/sh -c 
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
	
	$ docker wait MyName
	// 等待程序退出，得到返回代码
	
>

#### 在容器内部运行进程

>
	
	注意：
	在容器内运行的进程有两种类型，后台任务和交互任务
	后台任务在容器内运行且没有交互需求
	而交互任务则保持在前台运行 ...

	$ docker exec -d MyName touch /etc/new_config_file
	// 后台任务 ... 在容器内部额外启动新进程（后台任务） ...
	
	$ docker exec -t -i MyName /bin/bash
	// 交互任务 ... 在容器内部启动一个交互任务 ...
	// -t 模拟终端也是个交互程序 ...
	
	
>
	
#### 自动重启容器

>

	$ docker run --restart=always -d nljb.net ...
	// always 无论容器的退出代码是什么，都会自动重启该容器
	// on-failure 只有当容器的退出代码非0时，才会重启容器
	// --restart=on-failure:5，最多重启次数 ...
	
>

#### 宿主容器互传文件

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

#### 宿主目录挂在到容器

>

	$ docker run -it -v /nljb.net_mnt:/nljb.net_mnt nljb.net
	// 将宿主机的目录作为卷，挂在到容器里 ...
	
	$ docker run -it -v /nljb.net_mnt:/nljb.net_mnt:ro nljb.net
	// 这将使"容器"的nljb.net_mnt目录变成只读状态
	// 也就是说"宿主"可以修改内容，而容器不可以修改 ...
	
	* 希望同时对代码做开发和测试
	* 代码改动很频繁，不想在开发过程中重构镜像
	* 希望在多个容器间共享代码 ...
	
	注意：卷可以在容期间共享，即使容器停止，卷里的内容依然存在 ...

>

#### 暂停与恢复容器

>

	$ docker pause MyName
	// 暂停容器的所有进程
	// docker ps 中 STATUS 会变成 (Paused)
	
	$ docker unpause MyName
	// 恢复容器的所有进程
	
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

#### 容器互连

>

	$ docker run -p 4567 --name webapp --link redis:db  -it nljb.net /bin/bash
	// --link 标志创建了两个容器间的父子连接 ...
	// --link 参数：一个是要连接的容器名字，另一个是连接后的容器别名
	// 这个例子中：redis 是容器名字 db 是容器别名 ...
	
	// 注意到启动 redis 时没有使用 -p 标志公开 redis 端口
	// 因为不需要这么做，通过把容器连接到一起，可以让父容器直接访问任意子容器的公开端口
	// 更妙的是，容器端口不需要公开，只有 --link 标志连接到这个容器才能连接到这个端口
	// 所以：端口不需要对本地宿主公开，现在我们已经拥有一个非常安全的模型 ...
	
	$ docker run -p 4567 --name webapp2 --link redis:db ...
	$ docker run -p 4568 --name webapp3 --link redis:db ...
	...
	// 也可以把多个容器连接在一起 ... 比如多个Web应用容器连接到同一个 redis 服务 ...
	
>
	
	注意：
	
	// 在父容器中将(172.17.0.31 db)添加到(/etc/hosts)中 ...
	// 这样只要(ping db)就可以知道父容器是否与(子容器)连接 ...
	$ cat /ect/hosts
	172.17.0.33 811bd6d588cb 	
	// 容器自己的IP地址和主机名
	172.17.0.31 db				
	// redis 容器的IP地址和从该连接的别名衍生的主机名 db
	
	注意：还记得之前提到过，重启容器时，容器的IP地址会发生变化的事情吗 ...
	如果被连接的容器重启了，/etc/hosts文件中的IP地址会被新的IP地址更新 ...
	
>

#### 用于连接的环境变量

>

	$ root@811bd6d588cb:/ # env
	HOSTNAME=811bd6d588cb
	DB_NAME=/webapp/db
	DB_PORT_6379_TCP_PORT=6379
	DB_PORT=tcp://172.17.0.31:6379
	DB_PORT_6379_TCP=tcp://172.17.0.31:6379
	DB_ENV_REFRESHED_AT=2014-06-01
	DB_PORT_6379_TCP_ADDR=172.17.0.31
	DB_PORT_6379_TCP_PORTO=tcp
	...
	// 以 DB 开头是因为 DB 是创建连接时使用的别名
	
	这些自动创建的环境变量包含以下信息:
	* 子容器名称
	* 容器里运行的服务所使用的协议、IP和端口号
	* 容器里运行的不同服务所指定的协议、IP和端口号
	* 容器里由 Docker 设置的环境变量值 ...

>

#### 通过环境变量建立连接

>

	port = ENV['DB_PORT'] ...
	// 知道以上的环境变量信息，就可以在程序内通过环境变量获取DB端口号等

>

#### 容器内文件状态变化

>

	$ sudo docker diff 7bb0e258aefe
	C /dev
	A /dev/kmsg
	C /etc
	A /etc/mtab
	A /go
	A /go/src
	A /go/src/github.com
	A /go/src/github.com/dotcloud
	
	// diff 会列出容器内文件状态变化
	// A - Add, D - Delete, C - Change 

>

#### 导出和导入容器快照

>

	$ docker ps -a
	// 获取要导出的容器ID（7691a814370e）
	
	$ docker export 7691a814370e > nljb.net.tar
	// 从容器中导出快照 nljb.net.tar
	
	$ cat nljb.net.tar | sudo docker import - nljb:new
	// 将快照生成镜像 ... nljb:new 是一个新的镜像命名 ...

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
	$ docker images nljb.net
	// 列出本地镜像 ...
	// 本地镜像保存在/var/lib/docker目录下
	
	$ docker pull nljb.net 
	$ docker pull nljb.net:11.04
	// 拉取镜像 ...
	// 镜像是保存在 Docker Hub 中的，可以是官方的也可以是三方的
	
	$ docker search nljb.net 
	// 在 Docker Hub 中查找镜像
	
	$ docker tag ab035c88d533 nljb.net:11.04
	// 修改镜像标签 ...
	
	$ docker rmi nljb.net
	$ docker rmi `docker images -a -q	`
	// 删除镜像
	// 注意：不是容器，是镜像
	
	注意：
	我们虽然称之为nljb.net操作系统，但实际上它并不是完成的操作系统
	它只是一个裁剪版，由官方制作的，安全的裁剪版本 ...

>

#### 构建镜像

>

	* 使用 docker commit 命令
	* 使用 docker build 命令 和 Dockerfile 文件 ...

>

	// docker commit
	
	$ docker run -i -t nljb.net /bin/bash
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

#### 导出和导入镜像 ...

>

	$ docker save nljb:new > nljb.net.tar
	// 导出镜像到文件 ...
	
	$ docker load < nljb.net.tar
	$ docker load --input nljb.net.tar
	// 导入文件到镜像 ...
	
	注意：这样导出的镜像会非常大，因为包含所有镜像的增量备份 ...

>
	
#### 构建历史

>

	$ docker history nljb.net:nljb
	// 展示镜像的构建历史 ...
	
>

#### 历史记录

	$ docker images –tree
	// Docker的文件系统AUFS，一种“增量文件系统”
	// 用户所做修改以增量的方式保存，所以才能看到这些历史的增量。

>

#### 镜像其它

	$ docker run -d -p 80 nljb.net /bin/bash
	* 可以在宿主机器上随机选择49153～65535的端口号来映射到容器的80端口
	* 可以在宿主机器上指定一个具体的端口号来映射到容器的80端口上 ...

	$ docker run -d -p 8080(宿主):80(容器) nljb.net /bin/bash
	// 将“容器的80端口”绑定到“宿主机的8080端口”上 ...
	
	$ docker run -d -p 127.0.0.1:80(宿主):80(容器) nljb.net /bin/bash
	// 将容器的80端口绑定到宿主机的127.0.0.1这个IP的80端口上 ...

	$ docker run -d -p 127.0.0.1(宿主)::80(容器) nljb.net /bin/bash
	// 只指定宿主机的IP地址，没有端口号，则 docker 会随机指派端口号 ...
	
	$ docker run -d -P nljb.net /bin/bash
	// -P 该命令会将容器80端口对本地宿主机公开，并且绑定到宿主机的一个随机端口上
	// 有了这个端口号，就可以在宿主机中通过127.0.0.1连接到运行的容器查看Web了

	$ docker ps -l 
	// 可以看到包含端口映射的相关信息
	
	$ docker port f7cbdac22a02 80
	// 通过容器ID查看映射到宿主机80端口的容器端口号
	
>

#### 工作目录

>

	$ docker run -it -w /var/log nljb.net pwd
	// 该命令会更改容器内的工作目录 ...
	// 输出：/var/log
	
>

#### 环境变量

>
	
	$ docker run -it -e "GOPATH=/root/src" nljb.net env
	// -e 标志来传递环境变量, 这些变量只在运行时有效 ...
	// 输出：GOPATH=/root/src

>

#### 推送镜像 Docker Hub

>

	$ docker push youruser/yourimage
	// 将镜像推送到 Docker Hub 仓库

>