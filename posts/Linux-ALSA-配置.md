---
title: Linux ALSA 配置
date: '2014-07-04'
description:
categories:

tags:system

---

查看系统audio设备查看audio设备摘要信息

	aplay -l

	**** List of PLAYBACK Hardware Devices ****
	card 0: Intel [HDA Intel], device 0: ALC662 rev1 Analog [ALC662 rev1 Analog]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0
	card 0: Intel [HDA Intel], device 1: ALC662 rev1 Digital [ALC662 rev1 Digital]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0
	card 1: NVidia [HDA NVidia], device 3: HDMI 0 [HDMI 0]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0
	card 1: NVidia [HDA NVidia], device 7: HDMI 0 [HDMI 0]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0
	card 1: NVidia [HDA NVidia], device 8: HDMI 0 [HDMI 0]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0
	card 1: NVidia [HDA NVidia], device 9: HDMI 0 [HDMI 0]
	  Subdevices: 1/1
	  Subdevice #0: subdevice #0

查看audio详细信息

	aplay -L

可能输出

	null
	    Discard all samples (playback) or generate zero samples (capture)
	front:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    Front speakers
	surround40:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    4.0 Surround output to Front and Rear speakers
	surround41:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    4.1 Surround output to Front, Rear and Subwoofer speakers
	surround50:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    5.0 Surround output to Front, Center and Rear speakers
	surround51:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    5.1 Surround output to Front, Center, Rear and Subwoofer speakers
	surround71:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Analog
	    7.1 Surround output to Front, Center, Side, Rear and Woofer speakers
	iec958:CARD=Intel,DEV=0
	    HDA Intel, ALC662 rev1 Digital
	    IEC958 (S/PDIF) Digital Audio Output
	hdmi:CARD=NVidia,DEV=0
	    HDA NVidia, HDMI 0
	    HDMI Audio Output
	hdmi:CARD=NVidia,DEV=1
	    HDA NVidia, HDMI 0
	    HDMI Audio Output
	hdmi:CARD=NVidia,DEV=2
	    HDA NVidia, HDMI 0
	    HDMI Audio Output
	hdmi:CARD=NVidia,DEV=3
	    HDA NVidia, HDMI 0
	    HDMI Audio Output

配置文件,最简单的/etc/asound.conf格式如下:

	defaults.ctl.card 0
	defaults.pcm.card 0
	defaults.timer.card 0
	 
	pcm.!default {
		type hw
		card 0
		device 0
	}
	 
	ctl.!default {
		type hw
		card 0
		device 0
	}

	其中card和device的确定从aplay -l命令得到。对比aplaya -l的输出，可以看到上面的配置文件使用了Intel的音频设备。
	设置默认输出设备
	首先根据aplay -l的输出来确定声卡ID和设备ID
	把Intel模拟输出作为默认的audio输出设备

	defaults.ctl.card 0
	defaults.pcm.card 0
	defaults.timer.card 0
	  
	pcm.!default {
		type hw
		card 0
		device 0
	}
	  
	ctl.!default {
		type hw
		card 0
		device 0
	}

把Nvidia HDMI数字输出作为默认的audio输出设备

	defaults.ctl.card 1
	defaults.pcm.card 1
	defaults.timer.card 1
	  
	pcm.!default {
		type hw
		card 1
		device 7
	}
	  
	ctl.!default {
		type hw
		card 1
		device 7
	}

测试audio设备测试指定audio设备

	speaker-test -D front:Intel -c2 -r44100 -FS16_LE -twav

测试默认audio设备

	speaker-test -c2 -r44100 -FS16_LE -twav

调节audio设备

	alsamixer -c 0 <---[声卡编号]

配置文件的保存和还原

	alsactl store -f /var/lib/alsa/asound.state
	alsactl restore -f /var/lib/alsa/asound.state


