---
title: MBR-引导区文件
date: '2014-07-10'
description:
categories:

tags:system

---

查看硬盘分区信息

	parted /dev/sda print

备份MBR，linux下使用如下命令：

	dd if=/dev/hda of=/root/linux.bin bs=512 count=1

这里注意使用if=/dev/hda备份MBR中数据，如果grub安装具体某个分区，则要自己选择了。

写入mbr:

	dd if=/mnt/windows/linux.lnx of=/dev/hda bs=512 count=1

如果以上有什么不懂的可以在终端下输入 dd --help查看帮助。
