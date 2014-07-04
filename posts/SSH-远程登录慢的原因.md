---
title: SSH-远程登录慢的原因
date: '2014-07-04'
description:
categories:

tags:system

---

有时候在ssh远程登录到其他主机上时发现登录时间太长，要等待很久才会出现输入密码的提示，google了一下，发现主要有两个问题会导致ssh登录慢：

1.使用了dns反查，这样的话当ssh某个IP时，系统会试图通过DNS反查相对应的域名，如果DNS中没有这个IP的域名解析，则会等到DNS查询超时才会进行下一步，消耗很长时间。

	修改方式: vim /etc/ssh/sshd_config 增加一行记录：UseDNS no

	默认情况下会有一行被注释掉的记录#UseDNS yes，虽然这条记录被注释掉了，但ssh缺省情况下UseDNS的值是yes，所以要显式的指定该值为no。

	重新启动ssh服务
	远程登录会快很多。

2. 这种情况在本地主机或远程主机启动图形的情况下比较明显，该参数似乎是在做图形方面的认证，具体功能还不清楚，但修改以后可以明显提高ssh远程登录速度。

	vim /etc/ssh/sshd_config
	修改GSSAPIAuthentication参数为 no，默认是yes
	重新启动ssh服务
	 
ssh –vvv
  
	-vvv的参数可以查看ssh登录的过程，看看当前进行到了哪一步。


