---
title: udev-mount-umount
date: '2014-07-15'
description:
categories:

tags:system

---

	ACTION=="add|change", SUBSYSTEM=="block", BUS=="usb", KERNEL=="sd[a-z][0-9]", RUN+="/danoo/bin/addusb.sh %k"
	ACTION=="remove", SUBSYSTEM=="block", BUS=="usb", RUN+="/danoo/bin/removeusb.sh"


