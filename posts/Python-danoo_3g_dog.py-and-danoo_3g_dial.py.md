---
title: Python-danoo_3g_dog.py-and-danoo_3g_dial.py
date: '2014-07-04'
description:
categories:

tags:python 

---

	#!/usr/bin/env python
	import time,os
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
	DANOO_USB = get_link()
	DANOO_APN = get_dialer_list()
	print '>>>',DANOO_APN
	print '>>>',DANOO_USB
	if not len(DANOO_APN) == len(DANOO_USB):
	    print '>>>','wvdial.conf Config Error'
	    exit(0)
	while True:
	    WV_MEMBER = len(DANOO_APN)
	    print '>>> Dial Device Number',WV_MEMBER
	    for x in range(WV_MEMBER):
		try:
		    f = file(DANOO_USB[x])
		    f.close()
		except IOError:
		    print '>>> No such file or directory',DANOO_USB[x]
		    continue
		ST_WV = 'wvdial'+' '+DANOO_APN[x]
		ST_WVDIAL = os.system(ST_WV)
		print '>>>','Connect',DANOO_APN[x],'Error Sleep 10s'
		time.sleep(10)
	    print '>>>','Connect ALL Failure Sleep 30s Reconnect'
	    time.sleep(30)
	-----------------------------------------------  ↓ OLD  ↓ --------------------------------------------------------------------------
	#!/usr/bin/env python
	import time,os
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
	DANOO_USB = get_link()
	DANOO_APN = get_dialer_list()
	print '>>>',DANOO_APN
	print '>>>',DANOO_USB
	if not len(DANOO_APN) == len(DANOO_USB):
	    print '>>>','wvdial.conf Config Error'
	    exit(0)
	while True:
	    WV_MEMBER = len(DANOO_APN)
	    print '>>> Dial Device Number',WV_MEMBER
	    for x in range(WV_MEMBER):
		try:
		    f = file(DANOO_USB[x])
		    f.close()
		except IOError:
		    print '>>> No such file or directory',DANOO_USB[x]
		    continue
		ST_WV = 'wvdial'+' '+DANOO_APN[x]
		ST_WVDIAL = os.system(ST_WV)
		print '>>>','Connect',DANOO_APN[x],'Error Sleep 10s'
		time.sleep(10)
	    print '>>>','Connect ALL Failure Reload drive'
	    os.system('pkill wvdial')
	    os.system('pkill pppd')
	    time.sleep(10)
	    MOD_R = os.system('modprobe -r option usbserial')
	    MOD_A = os.system('modprobe option')
	    if MOD_R != 0:
		print '>>> Remove drive Error'
	    if MOD_A != 0:
		print '>>> Load drive Error'
	    print '>>>','Connect ALL Failure Sleep 30s Reconnect'
	    time.sleep(30)
	 
	----------------------------------------------- ↓ DOG ↓ --------------------------------------------------------------------------
	#def CHECK_WGET(w_addr,f_addr):
	# messaget = 0
	# try:
	# file = urllib.urlopen(w_addr)
	# messaget = file.read()
	# except IOError:
	# print '>>> urlopen %s error' % w_addr
	# file.close()
	# if messaget != 0:
	# messaget.index(f_addr)
	# messaget = 1
	# return messaget
	----------------------------------------------------------------------------------------------------------------------------------------------------
	#!/usr/bin/env python
	import os,time,subprocess
	CHECK_NET = ['www.baidu.com','www.google.cn','www.qq.com','www.msn.com']
	print '>>> Check Netwrok Address ->',CHECK_NET
	def shell_command(command):
	    command_info = subprocess.call(command,shell=True)
	    if command_info != 0:
		print '>>> command error -> ',command
	    return command_info
	def CHECK_PING(ping_add):
	    PING_ADD = 'ping -c 5'+' '+ping_add+' '+'>/dev/null'
	    PING_INFO = shell_command(PING_ADD)
	    return not PING_INFO
	def CHECK_TTYUSB(tty_usb):
	    TTY_INFO = 1
	    try:
		file(tty_usb)
	    except IOError:
		TTY_INFO = 0
	    return TTY_INFO
	def CHECK_TTYUSB_INFO():
	    global dial_pid
	    if CHECK_TTYUSB('/dev/ttyUSB0'):
		print '>>> Check /dev/ttyUSB0 OK'
	    elif CHECK_TTYUSB('/dev/ttyUSB2'):
		print '>>> Check /dev/ttyUSB2 OK'
	    else:
		print '>>> Check ttyUSB0 and ttyUSB2 Error -> No such file'
		print '>>> kill ',dial_pid.pid
		subprocess.Popen.kill(dial_pid)
		print '>>> Network Check False -> Recheck Start Reload Driver'
		print '>>> Network Check False -> Recheck Reload Danoo 3G Dial'
		if shell_command('pkill wvdial'):
		    print '>>> Recheck pkill wvdial Error'
		if shell_command('pkill pppd'):
		    print '>>> Recheck pkill pppd Error'
		if shell_command('modprobe -r option usbserial'):
		    print '>>> Recheck modprobe -r option usbserial Error'
		if shell_command('modprobe option'):
		    print '>>> Recheck modprobe option Error'
		print '>>> Network Check False -> Reload End'
		print '>>> Check ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> Start'
		time.sleep(3600)
		print '>>> Check ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck Start'
		NET_TTYS_RE = 0
		if CHECK_TTYUSB('/dev/ttyUSB0'):
		    print '>>> Recheck /dev/ttyUSB0 OK'
		elif CHECK_TTYUSB('/dev/ttyUSB2'):
		    print '>>> Recheck /dev/ttyUSB2 OK'
		else:
		    print '>>> Recheck ttyUSB0 and ttyUSB2 -> No such file'
		    NET_TTYS_RE = 0
		    print '>>> Recheck ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck End -> Reboot'
		    shell_command('reboot')
		if NET_TTYS_RE != 0:
		    print '>>> Recheck ttyUSB0 and ttyUSB2 -> Sleep 3600s Out Reboot -> End , Recheck End -> Find ttyUSB or Network Not Reboot'
	def START_DIAL():
	    global dial_pid
	    try:
		dial_pid = subprocess.Popen('/danoo/bin/danoo_3g_dial.py')
	    except IOSError:
		print '>>> /danoo/bin/danoo_3g_dial.py -> No such file'
		exit(1)
	    print '>>> /danoo/bin/danoo_3g_dial.py PID ->',dial_pid.pid
	#####################################################################
	START_DIAL()
	CHECK_TTYUSB_INFO()
	START_DIAL()
	print '>>> Danoo 3G DOG -> Time Sleep 1800s'
	time.sleep(1800)
	while True:
	    NET_INFO = 0
	    for x in range(len(CHECK_NET)):
		if CHECK_PING(CHECK_NET[x]):
		    NET_INFO = NET_INFO + 1
		    break
	    if not NET_INFO:
		CHECK_TTYUSB_INFO()
		print '>>> Network Check False -> Start Reload Driver'
		print '>>> Network Check False -> Start Reload Danoo 3G Dial'
		print '>>> kill ',dial_pid.pid
		subprocess.Popen.kill(dial_pid)
		if shell_command('pkill wvdial'):
		    print '>>> pkill wvdial Error'
		if shell_command('pkill pppd'):
		    print '>>> pkill pppd Error'
		if shell_command('modprobe -r option usbserial'):
		    print '>>> modprobe -r option usbserial Error'
		if shell_command('modprobe option'):
		    print '>>> modprobe option Error'
		START_DIAL()
		print '>>> Network Check False -> Reload End'
	    else:
		print '>>> Network Check True'
	    print '>>> Danoo 3G DOG -> Time Sleep 1800s'
	    time.sleep(1800)



