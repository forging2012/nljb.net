---
title: Ubuntu之 portmapper is not running 解决方案
date: '2017-07-20'
description:
categories:

tags:system

---

>

### Not starting: portmapper is not running

>

***以前叫portmap现在叫rpcbind, portmap的主要功能是把RPC程序号转化为Internet的端口号***

>

 * Stopping NFS kernel daemon                              [ OK ] 
 * Unexporting directories for NFS kernel daemon...        [ OK ] 
 * Exporting directories for NFS kernel daemon...          [ OK ] 
 * Starting NFS kernel daemon                                                                                                         
 * Not starting: portmapper is not running

>

	$ sudo apt-get purge rpcbind  
	$ sudo apt-get install nfs-kernel-server  

>

	$ sudo vim /etc/exports  
	/(your_share_name) *(rw,sync,no_subtree_check,no_root_squash)   

>

	$ sudo service rpcbind start  

>

	$ sudo /etc/init.d/nfs-kernel-server restart  

>
