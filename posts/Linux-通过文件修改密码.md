---
title: Linux-通过文件修改密码
date: '2014-07-04'
description:
categories:

tags:system

---

1,/etc/rc.d/rc.local 此文件可以写系统启动后开启的命令

	如 su – root -c ‘/home/mysql/ultserver/userver &’

2,/etc/profile 系统环境变量设置点

	export JAVA_HOME=/usr/java/jdk1.5.0_06
	export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JABA_HOME/lib/tools.jar

3,.bashrc 或 .bash_profile 设置用户而不是系统的环境变量

4,/etc/shadow 内容包括用户及被加密的密码以及其它/etc/passwd 不能包括的信息，比如用户的有效期限

	einan:$1$VE.Mq2Xf$2c9Qi7EQ9JP8GKF8gH7PB1:13072:0:99999:7:::
	linuxsir:$1$IPDvUhXP$8R6J/VtPXvLyXxhLWPrnt/:13072:0:99999:7::13108:

	第一字段：用户名（也被称为登录名），在/etc/shadow中，用户名和/etc/passwd 是相同的，这样就把passwd 和shadow
	中用的用户记录联系在一起；这个字段是非空的；
	第二字段：密码（已被加密），如果是有些用户在这段是x，表示这个用户不能登录到系统；这个字段是非空的；
	第三字段：上次修改口令的时间；这个时间是从1970年01月01日算起到最近一次修改口令的时间间隔（天数），
	您可以通过passwd 来修改用户的密码，然后查看/etc/shadow中此字段的变化；
	第四字段：两次修改口令间隔最少的天数；如果设置为0,则禁用此功能；也就是说用户必须经过多少天才能修改其口令；
	此项功能用处不是太大；默认值是通过/etc/login.defs文件定义中获取，PASS_MIN_DAYS 中有定义；
	第五字段：两次修改口令间隔最多的天数；这个能增强管理员管理用户口令的时效性，应该说在增强了
	系统的安全性；如果是系统默认值，是在添加用户时由/etc/login.defs文件定义中获取，在 
	PASS_MAX_DAYS 中定义；
	第六字段：提前多少天警告用户口令将过期；当用户登录系统后，系统登录程序提醒用户口令将
	要作废；如果是系统默认值，是在添加用户时由/etc/login.defs文件定义中获取，
	在PASS_WARN_AGE 中定义；
	第七字段：在口令过期之后多少天禁用此用户；此字段表示用户口令作废多少天后，
	系统会禁用此用户，也就是说系统会不能再让此用户登录，也不会提示用户过期，是完全禁用；
	第八字段：用户过期日期；此字段指定了用户作废的天数（从1970年的1月1日开始的天数），
	如果这个字段的值为空，帐号永久可用；
	第九字段：保留字段，目前为空，以备将来Linux发展之用；如果更为详细的，
	请用 man shadow来查看帮助，您会得到更为详尽的资料；

	/etc/password 帐户花名册

	beinan:x:500:500:beinan sun:/home/beinan:/bin/bash
	linuxsir:x:505:502:linuxsir open,linuxsir office,13898667715:/home/linuxsir:/bin/bash
	beinan:x:500:500:beinan sun:/home/beinan:/bin/bash
	linuxsir:x:501:502::/home/linuxsir:/bin/bash

	第一字段：用户名（也被称为登录名）；在上面的例子中，我们看到这两个用户的用户名分别是 
	beinan 和linuxsir；
	第二字段：口令；在例子中我们看到的是一个x，其实密码已被映射到/etc/shadow 文件中；
	第三字段：UID ；请参看本文的UID的解说；
	第四字段：GID；请参看本文的GID的解说；
	第五字段：用户名全称，这是可选的，可以不设置，在beinan这个用户中，用户的全称是beinan sun ；
	而linuxsir 这个用户是没有设置全称；
	第六字段：用户的家目录所在位置；beinan 这个用户是/home/beinan ，而linuxsir 这个用户是
	/home/linuxsir ；
	第七字段：用户所用SHELL 的类型，beinan和linuxsir 都用的是 bash ；所以设置为/bin/bash ； 

5,/etc/init.d/：所有服务的预设启动 script 都是放在这里的，例如要启动或者关闭 iptables 的话：

	/etc/init.d/iptables start 
	/etc/init.d/iptables stop

6./etc/inittab 设置启动level，如果要从图形界面启动切换到字符界面就需要修改这个文件


