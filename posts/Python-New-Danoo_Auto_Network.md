---
title: Python-New-Danoo_Auto_Network
date: '2014-07-04'
description:
categories:

tags:python 

---

	################### NetWorks WIFI Connect Mode ######################
	# NET_MODE -> "wep" or "wpa", cannot be "NULL"
	# NET_AUDH -> "dhcp" or "static", cannot be "NULL". WPA can only use dhcp.
	# NET_APAD -> AP Router IP address , cannot be "NULL"
	NET_MODE="wep"
	NET_AUDH="static"
	NET_SSID="danoowifi"
	NET_PASS="0175155789"
	NET_IPAD="192.168.10.133"
	NET_MASK="255.255.255.0"
	NET_GETW="192.168.10.1"
	NET_APAD="192.168.10.1"
	######################################################################

	其实可以通过find()搜索play_wifi文件内'='号位置后采集,然后将采集的位置加1即可,空值在bool中标识为假

--------------------------------------------------------------------------------------------------------------

	#!/usr/bin/env python
	import os,time
	def shell_command(command):
	    command_info = os.system(command)
	    if command_info != 0:
		print 'command error -> ',command
	    return command_info
	def config_file_command(command_member):
	    f = open('/root/.danoo_player_wifi','r')
	    str1 = f.readline().strip()
	    while len(str1):
		str1 = f.readline().strip()
		try:
		    file_str1 = str1.split('=')
		    file_member = file_str1.index(command_member)
		    return_str1 = file_str1[file_member+1].strip('"')  
		except ValueError:
		    continue
	    try:
		return return_str1
	    except UnboundLocalError:
		print '.danoo_player_wifi Config File Error'
		exit(0)
	    f.close()
	def device_info():
	    shell_command('ifconfig eth1 down')
	    if shell_command('ifconfig -a eth1') != 0:
		print 'eth1 Device not found -> exit'
		exit(0)
	    SHELL_ID = shell_command('ifconfig eth1 up')
	    ETH_LABLE_A=0
	    while SHELL_ID != 0:
		SHELL_ID = shell_command('ifconfig eth1 up')
		if ETH_LABLE_A == 1:
		    time.sleep(6)
		elif ETH_LABLE_A > 1 and ETH_LABLE_A < 6: 
		    time.sleep(10)
		elif ETH_LABLE_A == 6:
		    print 'eth1 Device UP Error -> exit'
		    exit(1)
		ETH_LABLE_A = ETH_LABLE_A + 1
	def eth_dhcp():
	    shell_command('dhclient -r eth1')
	    shell_command('dhclient eth1')
	def check_net():
	    PING_ADD = 'ping -c 3'+' '+NET_APAD
	    PING_INFO = shell_command(PING_ADD)
	    return PING_INFO
	if bool(config_file_command('NET_MODE')):
	    NET_MODE = config_file_command('NET_MODE')
	else:
	    print 'NET_MODE Config Error'
	    exit(1)
	if bool(config_file_command('NET_AUDH')):
	    NET_AUDH = config_file_command('NET_AUDH')
	else:
	    print 'NET_AUDH Config Error'
	    exit(1)
	if bool(config_file_command('NET_SSID')):
	    NET_SSID = config_file_command('NET_SSID')
	else:
	    print 'NET_SSID Config Error'
	    exit(1)
	if bool(config_file_command('NET_PASS')):
	    NET_PASS = config_file_command('NET_PASS')
	else:
	    print 'NET_PASS Config Error'
	    exit(1)
	NET_IPAD = config_file_command('NET_IPAD')
	NET_MASK = config_file_command('NET_MASK')
	if bool(config_file_command('NET_GETW')):
	    NET_GETW = config_file_command('NET_GETW')
	else:
	    print 'NET_GETW Config Error'
	    exit(1)
	if bool(config_file_command('NET_APAD')):
	    NET_APAD = config_file_command('NET_APAD')
	else:
	    print 'NET_APAD Config Error'
	    exit(1)
	def eth_dhcp():
		shell_command('dhclient -r eth1')
		shell_command('dhclient eth1')
	##################################################################
	CHECK_NET_INFO = check_net()
	while CHECK_NET_INFO:
	    if NET_MODE == 'wep':
		pass
		#########################################################################
	    elif NET_MODE == 'wpa':
		pass
		#########################################################################
	else:
	    print 'network ok'
	       

