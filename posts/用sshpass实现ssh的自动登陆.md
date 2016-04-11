---
title: 用sshpass实现ssh的自动登陆
date: '2014-07-04'
description:
categories:

tags:system

---

用sshpass实现ssh的自动登陆 

要实现ssh自动登录，网上搜了一下，主要有两种方法：

	1、生成公钥。
	2、编写expect脚本。这两种方法，用起来都有点复杂。

在新立得上安装ssh的时候，偶然发现一个sshpass，百度谷歌之，英文资料甚多，而中文资料寥寥。其实sshpass的用法很简单。

用法：

	    sshpass 参数 SSH命令(ssh，sftp，scp等)。
	    参数:
		-p password    //将参数password作为密码。
		-f passwordfile //提取文件passwordfile的第一行作为密码。
		-e        //将环境变量SSHPASS作为密码。

	    比如说：
		scp abc@192.168.0.5:/home/xxx/test /root   这个命令的作用是将服务器端文件test传到本地文件夹/root下。
		利用sshpass，假设密码为efghi，则可写作：
		ssh -p efghi scp abc@192.168.0.5:/home/xxx/test /root

另外，对于ssh的第一次登陆，会提示：“Are you sure you want to continue connecting (yes/no)”，这时用sshpass会不好使

	可以在ssh命令后面加上 -o StrictHostKeyChecking=no来解决。

比如说上面的命令，就可以写作ssh -p efghi scp abc@192.168.0.5:/home/xxx/test /root -o StrictHostKeyChecking=no。

