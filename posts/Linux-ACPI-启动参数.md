---
title: Linux ACPI 启动参数
date: '2014-08-12'
description:
categories:

tags:system

---

	[HW,ACPI,X86-64,i386]
	acpi={force|off|ht|strict|noirq}
	// ACPI的总开关

force表示强制启用；off表示强制禁用；

noirq表示不要将ACPI用于IRQ路由；

ht表示只运行足够的ACPI来支持超线程；

strict表示降低对不严格遵循ACPI规格的平台的兼容性。

>

	acpi_sleep={s3_bios,s3_mode}
	// ACPI休眠选项。

在从S3状态(挂起到内存)恢复的时候，硬件需要被正确的初始化。

这对大多数硬件都不成问题，除了显卡之外，因为显卡是由BIOS初始化的

内核无法获取必要的恢复信息(仅存在于BIOS中，内核无法读取)。

这个选项允许内核以两种方式尝试使用ACPI子系统来恢复显卡的状态。

>

	[HW,ACPI]
	acpi_sci={level|edge|high|low}
	// ACPI系统控制终端触发器模式
	// (System Control Interrupt trigger mode)。

	[HW,ACPI]
	acpi_irq_balance
	// 使ACPI对中断请求进行平衡，在APIC模式下为默认值

	acpi_irq_nobalance
	// ACPI不对中断请求进行平衡(默认)，PIC模式下为默认值

	// acpi_irq_pci=<irq>,<irq>...
	// 如果启用了irq_balance则将列出的中断号标记为已经被PCI子系统使用，可用于屏蔽某些中断。

>

	[HW,ACPI]
	acpi_os_name=

告诉ACPI BIOS操作系统的名称。
常常用来哄骗某些老旧的BIOS以为运行的是Windows系统。比如"Microsoft 2001"表示WinXP，"Microsoft Windows"表示Win98。

>

	[HW,ACPI]
	acpi_serialize

强制串行化ACPI机器语言(ACPI Machine Language)方法，操作系统使用这种语言与BIOS打交道。

>

	[HW,ACPI]
	acpi_use_timer_override

对于某些有毛病的 Nvidia NF5 主板需要使用此选项才能正常使用，不过此时 HPET 将失效。

>

	[IA-32,X86-64]
	acpi_pm_good 

跳过pmtimer的bug检测，强制内核假设这台机器的pmtimer没有毛病。用于解决某些有缺陷的BIOS。

>

	[KNL,ACPI]
	memmap=nn[KMG]#ss[KMG]

将从ss开始的nn长度的内存区域标记为ACPI数据。

	memmap=nn[KMG]$ss[KMG]

将从ss开始的nn长度的内存区域标记为"保留"。

>

	[ACPI]
	pnpacpi=off

禁用ACPI的即插即用功能，而使用PNPBIOS来代替。

>

	[HW,ACPI]
	processor.max_cstate={0|1|2|3|4|5|6|7|8|9}

无视ACPI表报告的值，强制制定CPU的最大C-state值。

这里的数字必须是一个有效的C-state值

比如Yonah处理器支持"0-4"五个级别：C0为正常状态，其他则为不同的省电模式(数字越大表示CPU休眠的程度越深/越省电)。
"9"表示超越所有的DMI黑名单限制。你的CPU的 95%的时间应该处于最深度的idle状态。

>

	processor.nocst

不使用_CST方法来侦测C-state值，而是使用传统的FADT方法。
