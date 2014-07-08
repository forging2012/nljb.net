---
title: Systemd-FAQ-简体中文
date: '2014-07-08'
description:
categories:

tags:system

---

Systemd FAQ (简体中文)

https://wiki.archlinux.org/index.php/Systemd_FAQ_(简体中文)

https://wiki.archlinux.org/index.php/Systemd_FAQ_%28%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87%29

http://blip.tv/linuxconfau/beyond-init-systemd-4715015   (Beyond init: systemd)


Contents

	1 常见问题
	1.1 Q: 为什么控制台上会显示日志信息？
	1.2 Q: 如何修改启用的可登陆的 tty 控制台（getty）数量？
	1.3 Q: 怎样输出更详细的开机信息？
	1.4 Q: 开机后控制台信息会被清空，如何避免？
	1.5 Q: 我不用官方内核，内核版本和编译参数有什么要注意的吗？
	1.6 Q: 怎样知道一个目标需要哪些进程服务？
	1.7 Q: 电脑关闭了但电源没有断。
	1.8 Q: 切换到 systemd 后，为什么 fakeRAID 没有挂载?
	1.9 Q: 如何在启动的时候，运行自定义的一个脚本？
	1.10 Q: .service 状态显示绿色的 "active (exited)" (例如 iptables)
	1.11 Q: Failed to issue method call: File exists 错误

---------------------

常见问题

Q: 为什么控制台上会显示日志信息？

	A:请自行设置内核日志等级（loglevel）
	以前，/etc/rc.sysinit 帮我们把 dmesg 的日志等级设置为 3，是比较合适的。
	内核参数中加入 loglevel=3 或 quiet 即可。

Q: 如何修改启用的可登陆的 tty 控制台（getty）数量？

	A:添加新的 getty：
	在 /etc/systemd/system/getty.target.wants/ 添加新的软链接即可：
	# ln -sf /usr/lib/systemd/system/getty@.service /etc/systemd/system/getty.target.wants/getty@tty9.service
	# systemctl start getty@tty9.service
	移除 getty：
	从 /etc/systemd/system/getty.target.wants/ 删除对应的软链接即可：
	# rm /etc/systemd/system/getty.target.wants/getty@tty5.service /etc/systemd/system/getty.target.wants/getty@tty6.service
	# systemctl stop getty@tty5.service getty@tty6.service
	用户也可以通过编辑/etc/systemd/logind.conf，将NAutoVTs修改为需要的 TTY 个数
	用这种方式，按需启动将会保持，而之前的方式将会在启动时就启动 TTY.
	systemd 不使用 /etc/inittab 文件。
	注意: 自 systemd 版本 30，系统默认只开启一个 getty
	只有切换到别的 tty 时，才会开启新的 getty（socket 激活式）
	但仍可使用上述方法强制添加新的 getty。

Q: 怎样输出更详细的开机信息？

	A:如果内核信息输出后就什么信息都不输出了，很可能是因为你在内核参数中添加了 quiet
	删除即可，然后你就可以看到一列列绿色的 [ OK ] 和红色的 [ FAILED ]了
	所有信息都记录在系统日志，可以通过 $ systemctl 查看系统状态，通过 journalctl 查看日志。

Q: 开机后控制台信息会被清空，如何避免？

	A:自己写一个 getty@tty1.service 文件 
	把/usr/lib/systemd/system/getty@.service复制到/etc/systemd/system/getty@tty1.service
	修改 TTYVTDisallocate 为 no.

Q: 电脑关闭了但电源没有断。

	A:使用 $ systemctl poweroff
	而不是 systemctl halt.

Q: 切换到 systemd 后，为什么 fakeRAID 没有挂载?

	A:请确保使用了
	# systemctl enable dmraid.service

Q: 如何在启动的时候，运行自定义的一个脚本？

	A:在/etc/systemd/system中新建一个文件(名称可以为 myscript.service) 
	然后在其中写入如下内容：

	[Unit]
	Description=My script

	[Service]
	ExecStart=/usr/bin/my-script

	[Install]
	WantedBy=multi-user.target 
	然后开启该守护进程
	# systemctl enable myscript.service
	本例是说当目标multi-usr载入的时候，会启动你这个自定义脚本。

Q: .service 状态显示绿色的 "active (exited)" (例如 iptables)

	A:这很正常，本例中的 iptables 并不是守护进程，而是由内核控制，所以装载完规则后自动退出了。
	通过下面命令检查规则是否正确加载 # iptables --list

Q: Failed to issue method call: File exists 错误

	A:此错误一般发生在systemctl enable创建系统连接到/etc/systemd/system/的时候
	一般是在切换显示管理器(例如从 GDM 到 KDM)时出现，这时/etc/systemd/system/display-manager.service 已经存在
	要解决此问题，使用 systemctl -f enable 覆盖原有链接。

---------------------

在之前是直接将 /etc/inittab 文件中id:5:initdefault 数字修改为  id:3:initdefault 就可以了.

现在再进入这个文件提示不再使用了.

需要修改 /etc/systemd/system/default.target 这个软连接文件.

这个软连接默认指向第5运行等级(GUI):

	/etc/systemd/system/default.target -> /lib/systemd/system/runlevel5.target

现在只要将这个软连接指向需要的运行等级就行了. 比如命令行界面多用户模式:

	cd /etc/systemd/system
	mv default.target runlevel5.target
	ln -s /lib/systemd/system/runlevel3.target default.target

这样使默认启动等级的target指向等级为3(CLI)的运行等级target.

	/etc/systemd/system/default.target -> /lib/systemd/system/runlevel3.target

然后重启系统就默认进入命令行多用户模式了,不会再启动GDM了.

如果想从GUI模式快速重新启动到命令行模式,可以直接使用命令 init 3 

下次如果想要改回去,只要把default.target改下名称,把runlevel5.target再改回default.target就行啦.

	mv default.target runlevel3.target
	mv runlevel5.target default.target

