---
title: Linux巨大变革之Systemd取代SysV的Init
date: '2015-04-17'
description:
categories:

tags:system

---

>

http://www.ibm.com/developerworks/cn/linux/1407_liuming_init3/index.html

>

### target 取代 runlevel

>

在systemd的管理体系里面，以前的运行级别（runlevel）的概念被新的运行目标（target）所取代。

注意：tartget的命名类似于multi-user.target等这种形式

比如原来的(runlevel3)就对应新的多用户目标(multi-user.target),(runlevel5)就相当于(graphical.target)。

>

	// 0 关闭系统。
	runlevel0.target, poweroff.target
	// 1, s, single 单用户模式。
	runlevel1.target, rescue.target	
	// 2,4 用户定义/域特定运行级别。默认等同于 3。
	runlevel2.target, runlevel4.target, multi-user.target
	// 3 多用户，非图形化。用户可以通过多个控制台或网络登录。
	runlevel3.target, multi-user.target
	// 5 多用户，图形化。通常为所有运行级别 3 的服务外加图形化登录。
	runlevel5.target, graphical.target
	// 6 重启
	runlevel6.target, reboot.target
	// emergency 紧急 Shell
	emergency.target

>

	注意：也就是(/etc/systemd/system)目录下(default.target -> /usr/lib/systemd/system/runlevel3.target)
	所以：runlevel3.target 对应 /etc/systemd/system/multi-user.target.wants 目录的 cron.service 
	同时：multi-user.target.wants/cron.service 也就是 cron.service -> /usr/lib/systemd/system/cron.service
	同样：runlevel5.target 对应 /etc/systemd/system/graphical.target.wants 
	备注：在openSUSE系统下可以使用 systemd 的主要命令行工具 systemctl 来管理。
	注意：在 systemd 下，有些系统服务器，service、chkconfig 兼容，有些系统则无法继续使用
	比如：在 chkconfig 运行级别3下执行 chkconfig sshd on 时，系统会自动在 multi-user.* 建立 sshd.service 连接
	原理：也就是将 /usr/lib/systemd/system/ 下对应的服务连接到 /etc/systemd/system/ 对应等级的目录中

>

	由于不再使用运行级别概念，所以/etc/inittab也不再被系统使用 
	而在systemd的管理体系里面，默认的target（相当于以前的默认运行级别）是通过软链来实现。如：
	ln -s /lib/systemd/system/runlevel3.target /etc/systemd/system/default.target
	在/lib/systemd/system/ 下面定义runlevelX.target文件目的主要是为了能够兼容以前的运行级别level的管理方法。
	事实上/lib/systemd/system/runlevel3.target，同样是被软连接到multi-user.target。
	注：opensuse下是在/usr/lib/systemd/system/目录下。

>

---

>

###  Systemd 命令和 sysvinit 命令的对照表

>

	Sysvinit 命令	        Systemd 命令	                备注
	service foo start	systemctl start foo.service	用来启动一个服务 (并不会重启现有的)
	service foo stop	systemctl stop foo.service	用来停止一个服务 (并不会重启现有的)。
	service foo restart	systemctl restart foo.service	用来停止并启动一个服务。
	service foo reload	systemctl reload foo.service	当支持时，重新装载配置文件而不中断等待操作。
	service foo condrestart	systemctl condrestart foo.service	如果服务正在运行那么重启它。
	service foo status	systemctl status foo.service	        汇报服务是否正在运行。
	ls /etc/rc.d/init.d/	systemctl list-unit-files --type=service	用来列出可以启动或停止的服务列表。
	chkconfig foo on	systemctl enable foo.service	在下次启动时或满足其他触发条件时设置服务为启用
	chkconfig foo off	systemctl disable foo.service	在下次启动时或满足其他触发条件时设置服务为禁用
	chkconfig foo		systemctl is-enabled foo.service	用来检查一个服务在当前环境下被配置为启用还是禁用。
	chkconfig –list		systemctl list-unit-files --type=service	输出在各个运行级别下服务的启用和禁用情况
	chkconfig foo –list	ls /etc/systemd/system/*.wants/foo.service	用来列出该服务在哪些运行级别下启用和禁用。
	chkconfig foo –add	systemctl daemon-reload	                        当您创建新服务文件或者变更设置时使用。
	telinit 3		systemctl isolate multi-user.target (OR systemctl isolate runlevel3.target OR telinit 3) 改变至多用户运行级别。

>

---

>

### Systemd 电源管理命令

>

	systemctl reboot	重启机器
	systemctl poweroff	关机
	systemctl suspend	待机
	systemctl hibernate	休眠
	systemctl hybrid-sleep	混合休眠模式（同时休眠到硬盘并待机）

>

---

>

### Systemd 物理文件组成

>

Systemd是一个完整的软件包，安装完成后有很多物理文件组成.

大致分布为，配置文件位于/etc/systemd这个目录下，配置工具命令位于/bin，和/sbin这两个目录下

预先准备的备用配置文件位于/lib/systemd目录下，还有库文件和帮助手册等等。

这是一个庞大的软件包。详情使用rpm -ql systemd即可查看。

>

---

>

### Systemd 运行原理

>

配置单元(unit)

>

系统初始化要做很多工作，如挂载文件系统，启动服务，配置交换分区，都可以看做是一个配置单元，安装功能不同把配置单元分成多种类型。

>

	service 
	// 后台服务进程，如httpd，mysqld等
	soket 
	// 对应一个套接字，之后对应到一个service，类似于xinetd的功能
	device 
	// 对应udev规则标记的一个设备
	mount 
	// 系统中的一个挂载点，systemd据此进行自动挂载，为了与SystemV兼容，目前systemd自动处理/etc/fstab并转化为mount
	automount 
	// 自动挂载点
	swap 
	// 配置交换分区
	target 
	// 配置单元的逻辑分组，包含多个相关的配置单元，可以当成是SystemV中的运行级。
	timer 
	// 定时器。用来定时触发用户定义的操作，它可以用来取代传统的atd，crond等。
	snapshot 
	// 与target类似，表示当前的运行状态

	注意：每一个配置单元都有一个对应的配置文件，系统管理员的任务就是处理这些不同的文件，比如一个MySql服务对应一个mysql.service文件。

>

---

>

### Journald

>

Journald是systemd独有的日志系统，替换了sysVinit中的syslog守护进程。命令journalctl用来读取日志。

>

	// 所有引导日志
	journalctl -b
	// 系统时时日志（tail -f)
	journalctl -f
	// 指定查看程序日志
	journalctl /usr/sbin/dnsmasq

>

---

>

### hostnamectl

>

Systemd带来了一整套与操作系统交互的新途径，并且极具特色。

举个栗子，你可以用hostnamectl命令来获得你的linux机器的hostname和其它有用的独特信息。

	   Static hostname: danooplayer
		 Icon name: computer-desktop
		   Chassis: desktop
		Machine ID: 0635b925bb7b4b7d9ad909b98374199d
		   Boot ID: be2f99093bd3442c9c9516b938389c16
	  Operating System: openSUSE 13.1 (Bottle) (i586)
	       CPE OS Name: cpe:/o:opensuse:opensuse:13.1
		    Kernel: Linux 3.11.6-4-desktop
	      Architecture: i686

>

---

>

### Systemctl

>

	// 列出当前目标
	systemctl list-units --type=target
		// 输出
		basic.target         loaded active active Basic System
		cryptsetup.target    loaded active active Encrypted Volumes
		getty.target         loaded active active Login Prompts
		local-fs-pre.target  loaded active active Local File Systems (Pre)
		local-fs.target      loaded active active Local File Systems
		multi-user.target    loaded active active Multi-User System
		network.target       loaded active active Network
		nss-lookup.target    loaded active active Host and Network Name Lookups
		paths.target         loaded active active Paths
		remote-fs-pre.target loaded active active Remote File Systems (Pre)
		remote-fs.target     loaded active active Remote File Systems
		slices.target        loaded active active Slices
		sockets.target       loaded active active Sockets
		swap.target          loaded active active Swap
		sysinit.target       loaded active active System Initialization
		timers.target        loaded active active Timers
	// 改变当前目标
	systemctl isolate graphical.target
		// 查看支持的 target 
		ll /usr/lib/systemd/system/*.target
	// 默认目标
	systemctl get-default
		// 输出
		runlevel3.target
	// 设置默认目标
	systemctl set-default graphical.target / multi-user.target
	// 使某服务自动启动	
	systemctl enable httpd.service
	// 使某服务不自动启动	
	systemctl disable httpd.service
	// 检查服务状态	
	systemctl status httpd.service （服务详细信息） 
	systemctl is-active httpd.service （仅显示是否 Active)
	// 显示所有已启动的服务	
	systemctl list-units --type=service
	systemctl list-units --type=target
	// 启动某服务
	systemctl start httpd.service
	// 停止某服务
	systemctl stop httpd.service
	// 重启某服务
	systemctl restart httpd.service

>

---

>

### Systemd 的使用

>

下面针对技术人员的不同角色来简单地介绍一下 systemd 的使用。

本文只打算给出简单的描述，让您对 systemd 的使用有一个大概的理解。

具体的细节内容太多，即无法在一篇短文内写全，本人也没有那么强大的能力。

还需要读者自己去进一步查阅 systemd 的文档。

>

系统软件开发人员

开发人员需要了解 systemd 的更多细节。

比如您打算开发一个新的系统服务，就必须了解如何让这个服务能够被 systemd 管理。

这需要您注意以下这些要点：

	后台服务进程代码不需要执行两次派生来实现后台精灵进程，只需要实现服务本身的主循环即可。
	不要调用 setsid()，交给 systemd 处理
	不再需要维护 pid 文件。
	Systemd 提供了日志功能，服务进程只需要输出到 stderr 即可，无需使用 syslog。
	处理信号 SIGTERM，这个信号的唯一正确作用就是停止当前服务，不要做其他的事情。
	SIGHUP 信号的作用是重启服务。
	需要套接字的服务，不要自己创建套接字，让 systemd 传入套接字。
	使用 sd_notify()函数通知 systemd 服务自己的状态改变。一般地，当服务初始化结束，进入服务就绪状态时，可以调用它。

>

Unit 文件的编写

对于开发者来说，工作量最大的部分应该是编写配置单元文件，定义所需要的单元。

举例来说，开发人员开发了一个新的服务程序

比如 httpd，就需要为其编写一个配置单元文件以便该服务可以被 systemd 管理

类似 UpStart 的工作配置文件。在该文件中定义服务启动的命令行语法，以及和其他服务的依赖关系等。

此外我们之前已经了解到，systemd 的功能繁多，不仅用来管理服务，还可以管理挂载点，定义定时任务等。

这些工作都是由编辑相应的配置单元文件完成的。我在这里给出几个配置单元文件的例子。

下面是 SSH 服务的配置单元文件，服务配置单元文件以.service 为文件名后缀。

>

	  #cat /etc/system/system/sshd.service
	  [Unit]
	  Description=OpenSSH server daemon
	  [Service]
	  EnvironmentFile=/etc/sysconfig/sshd
	  ExecStartPre=/usr/sbin/sshd-keygen
	  ExecStart=/usrsbin/sshd –D $OPTIONS
	  ExecReload=/bin/kill –HUP $MAINPID
	  KillMode=process
	  Restart=on-failure
	  RestartSec=42s
	  [Install]
	  WantedBy=multi-user.target

>

文件分为三个小节。

	第一个是[Unit]部分，这里仅仅有一个描述信息。
	第二部分是 Service 定义，其中:
		ExecStartPre 定义启动服务之前应该运行的命令；
		ExecStart 定义启动服务的具体命令行语法。
	第三部分是[Install]，WangtedBy 表明这个服务是在多用户模式下所需要的。

>

那我们就来看下 multi-user.target 吧：

	  #cat multi-user.target
	  [Unit]
	  Description=Multi-User System
	  Documentation=man.systemd.special(7)
	  Requires=basic.target
	  Conflicts=rescue.service rescure.target
	  After=basic.target rescue.service rescue.target
	  AllowIsolate=yes
	  [Install]
	  Alias=default.target

第一部分中的 Requires 定义表明 multi-user.target 启动的时候 basic.target 也必须被启动；

另外 basic.target 停止的时候，multi-user.target 也必须停止。

如果您接着查看 basic.target 文件，会发现它又指定了 sysinit.target 等其他的单元必须随之启动。

同样 sysinit.target 也会包含其他的单元。

采用这样的层层链接的结构，最终所有需要支持多用户模式的组件服务都会被初始化启动好。

>

此外在/etc/systemd/system 目录下还可以看到诸如*.wants 的目录

放在该目录下的配置单元文件等同于在[Unit]小节中的 wants 关键字

即本单元启动时，还需要启动这些单元。

比如您可以简单地把您自己写的 foo.service 文件放入 multi-user.target.wants 目录下，这样每次都会被默认启动了。

>

最后，让我们来看看 sys-kernel-debug.mout 文件，这个文件定义了一个文件挂载点：

	#cat sys-kernel-debug.mount
	[Unit]
	Description=Debug File Syste
	DefaultDependencies=no
	ConditionPathExists=/sys/kernel/debug
	Before=sysinit.target
	[Mount]
	What=debugfs
	Where=/sys/kernel/debug
	Type=debugfs

这个配置单元文件定义了一个挂载点。

挂载配置单元文件有一个[Mount]配置小节，里面配置了 What，Where 和 Type 三个数据项。

这都是挂载命令所必须的，例子中的配置等同于下面这个挂载命令：

mount –t debugfs /sys/kernel/debug debugfs

配置单元文件的编写需要很多的学习，必须参考 systemd 附带的 man 等文档进行深入学习。

希望通过上面几个小例子，大家已经了解配置单元文件的作用和一般写法了。

