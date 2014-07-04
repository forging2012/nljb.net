---
title: Linux-启动时显示详细启动过程
date: '2014-07-03'
description:
categories:

tags:system

---

*. 修改/boot/grub/menu.lst，修改kernel...那行，将后面的quiet 去掉

*. 若想显示更信息，还可将splash=silent改为splash=verbose

*. 例如：

	kernel /boot/vmlinuz-2.6.31.14-0.8-desktop root=/dev/disk/by-id/ata-WDC_WD10EALS-00Z8A0_WD-WCATR4066961-part2 resume=/dev/disk/by-id/ata-WDC_WD10EALS-00Z8A0_WD-WCATR4066961-part1 splash=silent quiet showopts vga=0x31a

*. 改为：

	kernel /boot/vmlinuz-2.6.31.14-0.8-desktop root=/dev/disk/by-id/ata-WDC_WD10EALS-00Z8A0_WD-WCATR4066961-part2 resume=/dev/disk/by-id/ata-WDC_WD10EALS-00Z8A0_WD-WCATR4066961-part1 splash=verbose showopts vga=0x31a

