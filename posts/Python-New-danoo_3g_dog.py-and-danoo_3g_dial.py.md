---
title: Python-New-danoo_3g_dog.py-and-danoo_3g_dial.py
date: '2014-07-04'
description:
categories:

tags:python

---

-------------------------------------------------------------------------------------------------------------------------
	 
	#!/usr/bin/env python
	import time,os,sys,commands
	def get_link():
	    f = open("/etc/wvdial.conf","r")
	    str_end = []
	    for str in f:
		try:
		    str = str.strip()
		    str.index('Modem')
		    str_member = str[str.index('=')+1:].strip()
		    str_end.append(str_member)
		except ValueError:
		    continue
	    f.close()
	    return str_end[:]
	def get_dialer_list():
	    af = open("/etc/wvdial.conf","r")
	    str_end = []
	    for str in af:
		try:
		    str = str.strip()
		    str.index('Dialer')
		    str_member = str[str.index('[Dialer')+7:-1].strip()
		    str_end.append(str_member)
		except ValueError:
		    continue
	    af.close()
	    return str_end[:]
	def print_log(log_head='',log_info=''):
	    f_log = open('/var/log/danoo_3G_dial.log','a+')
	    print log_head,log_info
	    print >>f_log,log_head,log_info
	    f_log.close()
	try:
	    file("/etc/wvdial.conf","r")
	except IOError:
	    print_log('>>>','wvdial.conf File Error')
	    exit(1)
	file('/var/log/danoo_3G_dial.log','w')
	DANOO_USB = get_link()
	DANOO_APN = get_dialer_list()
	print_log('>>>',DANOO_APN)
	print_log('>>>',DANOO_USB)
	if not len(DANOO_APN) == len(DANOO_USB):
	    print_log('>>>','wvdial.conf Config Error')
	    exit(1)
	while True:
	    WV_MEMBER = len(DANOO_APN)
	    print_log('>>> Dial Device Number',WV_MEMBER)
	    for x in range(WV_MEMBER):
		try:
		    f = file(DANOO_USB[x])
		    f.close()
		except IOError:
		    print_log('>>> No such file or directory',DANOO_USB[x])
		    continue
		ST_WV = 'wvdial'+' '+DANOO_APN[x]
		ST_INFO_L,ST_WVDIAL = commands.getstatusoutput(ST_WV)
		print_log(ST_WVDIAL)
		print_log('>>> Connect Error Sleep 10s',DANOO_APN[x])
		time.sleep(10)
	    print_log('>>>','Connect ALL Failure Sleep 30s Reconnect')
	    time.sleep(30)
	-------------------------------------------------------------------------------------------------------------------------
								   分界线
	-------------------------------------------------------------------------------------------------------------------------
	 
	#!/usr/bin/env python
	import os,time,subprocess,urllib2,socket
	# wget  timeout
	socket.setdefaulttimeout(30)
	def print_log(log_head='',log_info=''):
	    f_log = open('/var/log/danoo_3G_dog.log','a+')
	    print log_head,log_info
	    print >>f_log,log_head,log_info
	    f_log.close()
	file('/var/log/danoo_3G_dog.log','w')
	CHECK_NET = ['http://www.baidu.com','http://www.google.com','http://cn.bing.com']
	CHECK_USB = ['/dev/ttyUSB0','/dev/ttyUSB2']
	print_log('>>> Check Netwrok Address ->',CHECK_NET)
	print_log('>>> Check HuaWei USB Device ->',CHECK_USB)
	def shell_command(command):
	    command_info = subprocess.call(command,shell=True)
	    if command_info != 0:
		print_log('>>> command error -> ',command)
	    return command_info
	def CHECK_NETWORK():
	    wget_member = 0
	    for x in CHECK_NET:
		try:
		    open_get = urllib2.urlopen(x)
		except urllib2.URLError:
		    continue
		open_file = open_get.read()
		if len(open_file) != 0:
		    wget_member += 1
		open_get.close()
	    if wget_member > 0:
		print_log('>>> Check Network Connection Is Working')
		print_log('>>> Network Connection Working -> CHECK_3G_INFO -> 0')
		f_l = file('/tags/check_3g_info','w')
		f_l.write('0')
		f_l.close
		net_info = True
	    else:
		print_log('>>> Check Network Connection Fails')
		net_info = False
	    return net_info
	def CHECK_MEMBER():
	    try:
		f = file('/tags/check_3g_info','r')
	    except IOError:
		f = file('/tags/check_3g_info','w+r')
		f.write('0')
	    f.seek(0)
	    CHECK_3G_INFO = f.read()
	    f.close
	    return CHECK_3G_INFO
	def CHECK_TTYUSB():
	    usb_member = 0
	    for u in CHECK_USB:
		try:
		    file(u)
		except IOError:
		    usb_member += 1
		    continue
	    if usb_member != 2:
		print_log('>>> Check ttyUSB0 and ttyUSB2 OK -> Such Device')
		usb_info = True
	    else:
		print_log('>>> Check ttyUSB0 and ttyUSB2 Error -> No Such Device')
		usb_info = False
	    return usb_info
	def CHECK_RETTYUSB():
	    if CHECK_TTYUSB() == False:
		print_log('>>> No Such ttyUSB0 and ttyUSB2 Device -> Reload Drive')
		if shell_command('modprobe -r option usbserial'):
		    print_log('>>> Reload modprobe -r option usbserial Error')
		else:
		    print_log('>>> Reload modprobe -r option usbserial OK')
		if shell_command('modprobe option'):
		    print_log('>>> Reload modprobe option Error')
		else:
		    print_log('>>> Reload modprobe option OK')
		if CHECK_TTYUSB() == False :
		    print_log('>>> No Such ttyUSB0 and ttyUSB2 Device -> Reload Drive Error')
		    print_log('>>> Check ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> Start')
		    time.sleep(3600)
		    print_log('>>> Check ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck Start')
		    CHECK_3G_INFO = int(CHECK_MEMBER())
		    print_log('>>> Check Reboot Member -> CHECK_3G_INFO ->',CHECK_3G_INFO)
		    if CHECK_TTYUSB() == False and CHECK_NETWORK() == False and CHECK_3G_INFO < 3:
			print_log('>>> Recheck ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck End -> Reboot')
			print_log('>>> Reboot System -> CHECK_3G_INFO + 1')
			f_e = file('/tags/check_3g_info','w')
			f_e.write(str(CHECK_3G_INFO+1))
			f_e.close()
			shell_command('reboot')
		    else:
			print_log('>>> Recheck ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck End -> Find ttyUSB or Network Not Reboot or CHECK_MEMBER()')
		else:
		    print_log('>>> No Such ttyUSB0 and ttyUSB2 Device -> Reload Drive OK')
		  
	def START_DIAL():
	    global dial_pid
	    try:
		dial_pid = subprocess.Popen('./danoo_3g_dial')
	    except IOError:
		print_log('>>> danoo_3g_dial -> No such file')
		exit(1)
	    print_log('>>> danoo_3g_dial PID ->',dial_pid.pid)
	# check usb device
	CHECK_RETTYUSB()
	# start wvdial
	START_DIAL()
	print_log('>>> Danoo 3G DOG -> Time Sleep 1800s')
	time.sleep(1800)
	while True:
	    if CHECK_NETWORK() == False:
		print_log('>>> Network Check False -> Recheck Device')
		CHECK_RETTYUSB()
		print_log('>>> kill ',dial_pid.pid)
		subprocess.Popen.kill(dial_pid)
		if shell_command('pkill wvdial'):
		    print_log('>>> pkill wvdial Error')
		if shell_command('pkill pppd'):
		    print_log('>>> pkill pppd Error')
		print_log('>>> Network Check False -> Start Reload Driver')
		if shell_command('modprobe -r option usbserial'):
		    print_log('>>> modprobe -r option usbserial Error')
		else:
		    print_log('>>> modprobe -r option usbserial OK')
		if shell_command('modprobe option'):
		    print_log('>>> modprobe option Error')
		else:
		    print_log('>>> modprobe option OK')
		print_log('>>> Network Check False -> Start Reload Danoo 3G Dial')
		START_DIAL()
	    else:
		print_log('>>> Network Check True')
	    print_log('>>> Danoo 3G DOG -> Time Sleep 1800s')
	    time.sleep(1800)  
	-------------------------------------------------------------------------------------------------------------------------

