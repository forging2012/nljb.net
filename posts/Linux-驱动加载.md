---
title: Linux-驱动加载
date: '2014-07-04'
description:
categories:

tags:system

---

    一般使用 modprobe ，因为 insmod 不考虑 mod 的模块依赖问题。

    modprobe 会检索模块依赖关系后载入依赖的模块（当然并不是绝对都能……）

    不过 modprobe 只能载入 /lib/modules/<kernel ver>/ 里面的模块。而且这个模块必须在这个目录里面的配置文件里面注册了

    使用分析可载入模块的相依性 depmod -a

    删除模块可以使用rmmod xxx 或者 modprobe -r  xxx 即可

  　depmod可检测模块的相依性，供modprobe在安装模块时使用。

    /etc/modprobe.d/50-blacklist.conf   --->   blacklist vt6656_stage

----------------------------------------------------------------------------------------

	　depmod [-adeisvV][-m <文件>][--help][模块名称]
	    -a或--all 分析所有可用的模块。
	　　-d或debug 执行排错模式。
	　　-e 输出无法参照的符号。
	　　-i 不检查符号表的版本。
	　　-m<文件>或system-map<文件> 使用指定的符号表文件。
	　　-s或--system-log 在系统记录中记录错误。
	　　-v或--verbose 执行时显示详细的信息。
	　　-V或--version 显示版本信息。
	　　--help 显示帮助。

----------------------------------------------------------------------------------------

