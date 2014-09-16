---
title: PXE引导纯内存OpenSUSE方案
date: '2014-07-03'
description:
categories:

tags:system

---

	mknod -m 0660 /dev/loop0 b 7 0

---

	emergency() {
	    local plymouth sulogin
	    if plymouth=$(type -p plymouth 2> /dev/null) ; then
		$plymouth quit
		$plymouth --wait
	    fi
	    if test -w /proc/splash ; then
		echo verbose >| /proc/splash
	    fi
	    cd /
	    echo -n "${1+$@} -- "
	    if sulogin=$(type -p sulogin 2> /dev/null); then
		echo "exiting to $sulogin"
		PATH=$PATH PS1='$ ' $sulogin /dev/console
	    else
		echo "exiting to /bin/sh"
		PATH=$PATH PS1='$ ' /bin/sh -i
	    fi
	}

---

	// FTP 获取镜像方式（实现无盘)
	echo "#################################"

	echo "Server ->" ${addr}

	/usr/bin/ftp -nv ${addr} <<!
	user anonymous anonymous
	prompt off
	hash
	bin
	lcd /
	mget image
	close
	!

	/sbin/depmod -a
	sleep 1

	/sbin/modprobe loop
	sleep 1

	/sbin/losetup /dev/loop0 /image && ls -l /dev/loop0
	sleep 1

	/bin/mount /dev/loop0 /root
	sleep 1

	echo "#################################"

---

	// git@github.com:nulijiabei/boot.git

	// 注：image为根目录打包镜像,不要超过2G,需要LOOP驱动

	echo "#################################"
	 
	root=/dev/loop0
	rootdev=/dev/loop0
	 
	echo " "
	echo "NEW ROOT IS root="$root""
	echo "NEW ROOTDEV IS rootdev="$rootdev""
	 
	echo "depmod"
	/sbin/depmod -a
	 
	echo "modprobe"
	/sbin/modprobe loop
	 
	echo "ls"
	ls -l /dev/loop0
	 
	echo "mount"
	/bin/mount /dev/sda1 /disk
	sleep 5
	 
	echo "CP"
	/bin/cp -av /disk/image /image && ls -l /image
	sleep 5
	 
	echo "umount"
	/bin/umount /disk
	sleep 5
	 
	echo "losetup"
	/sbin/losetup /dev/loop0 /image
	sleep 5
	 
	echo "mount"
	/bin/mount /dev/loop0 /root
	sleep 5
	 
	echo "ls"
	ls -l /root
	 
	echo "#################################"


	// 配置项

	// --------------------------------------------- //
	 
	保证工作目录(777)权限
	 
	// --------------------------------------------- //
	 
	安装 vsftpd
	安装 tftp
	安装 dhcp
	安装 dhcp-server
	安装 yast2-tftp-server
	 
	// --------------------------------------------- //
	 
	// --------------------------------------------- //
	 
	在 /etc/vsftpd.conf 增加
	 
	write_enable=NO
	dirmessage_enable=YES
	nopriv_user=ftpsecure
	local_enable=YES
	anonymous_enable=YES
	anon_world_readable_only=YES
	syslog_enable=NO
	connect_from_port_20=YES
	ascii_upload_enable=YES
	pam_service_name=vsftpd
	ssl_enable=NO
	pasv_min_port=30000
	pasv_max_port=30100
	listen=YES
	anon_mkdir_write_enable=NO
	anon_root=/srv/tftpboot
	anon_upload_enable=NO
	chroot_local_user=NO
	ftpd_banner=Welcome message
	idle_session_timeout=900
	local_root=/srv/tftpboot
	log_ftp_protocol=NO
	max_clients=10
	max_per_ip=3
	pasv_enable=YES
	ssl_sslv2=NO
	ssl_sslv3=NO
	ssl_tlsv1=YES
	 
	// --------------------------------------------- //
	 
	// --------------------------------------------- //
	 
	配置 /etc/dhcpd.conf
	 
	# dhcpd.conf
	option domain-name-servers 202.106.0.20;
	 
	ddns-update-style interim;
	 
	ignore client-updates;
	 
	allow booting;
	allow bootp;
	filename "pxelinux.0";
	 
	default-lease-time 1800;
	max-lease-time 7200;
	 
	subnet 192.168.0.0 netmask 255.255.255.0 {
	    option broadcast-address 192.168.0.255;
	    server-name "install-server";
	    range 192.168.0.10 192.168.0.224;
	    option routers 192.168.0.1;
	}

	// 指定IP地址
	host danooplayer {
	       hardware ethernet 00:E0:B4:0B:DD:70;
	       fixed-address 192.168.2.200;
	}	
	 
	// --------------------------------------------- //
	 
	// --------------------------------------------- //
	 
	配置 /etc/sysconfig/dhcpd
	 
	DHCPD_INTERFACE="eth0"
	DHCPD6_INTERFACE="eth0"
	DHCPD_IFUP_RESTART=""
	DHCPD6_IFUP_RESTART=""
	DHCPD_RUN_CHROOTED="yes"
	DHCPD6_RUN_CHROOTED="yes"
	DHCPD_CONF_INCLUDE_FILES=""
	DHCPD6_CONF_INCLUDE_FILES=""
	DHCPD_RUN_AS="dhcpd"
	DHCPD6_RUN_AS="dhcpd"
	DHCPD_OTHER_ARGS=""
	DHCPD6_OTHER_ARGS=""
	DHCPD_BINARY=""
	DHCPD6_BINARY=""
	 
	// --------------------------------------------- //
	 
	配置 /etc/xinetd.d/tftp
	 
	service tftp
	{
		socket_type     = dgram
		protocol        = udp
		wait            = yes
		flags           = IPv4
		user            = root
		server          = /usr/sbin/in.tftpd
		server_args     =  -s /srv/tftpboot
		disable         = no
	}
	 
	// --------------------------------------------- //
	 
	配置 Yast 中 TFTP
	 
	Enable
	/srv/tftpboot
	 
	// --------------------------------------------- //
	 
	配置 /srv/tftpboot 及镜像
	 
	目录:
	pxelinux.0
	pxelinux.cfg/default
	danoo/vmlinuz
	danoo/tinycore.gz
	 
	文件 pxelinux.cfg/default
	 
	default linux
	prompt 1
	timeout 60
	 
	label linux
	kernel danoo/vmlinuz
	append ip=dhcp initrd=danoo/tinycore.gz
	 
	 
	// --------------------------------------------- //
