---
title: Linux ACPI 启动参数
date: '2014-08-12'
description:
categories:

tags:system

---

[acpi]

>

	acpi= [HW,ACPI] Advanced Configuration and Power Interface

	force : 强制开启acpi.
	off   : 关闭acpi.
	noirq : 不要使用acpi去路由irq。禁止使用ACPI来处理PCI设备中断路由
			和pci=noacpi的区别是它允许使用ACPI来枚举PCI root bus.
	ht    : 只使用足够的acpi去支持超线程
			这个参数和"acpi=off"几乎一样，它禁止了除多处理器配置相关的内容以外的所有ACPI功能。
			如果acpi=off正常，但acpi=ht不正常, 则解析ACPI表或者Linux SMP的代码有bug.
	strict : 降低对不严格遵循acpi规范平台的兼容性

>

	// ACPI休眠选项。
	acpi_sleep= [HW,ACPI] Sleep options

	Format: { s3_bios, s3_mode }

在从S3状态(挂起到内存)恢复的时候，硬件需要被正确的初始化。

这对大多数硬件都不成问题，除了显卡之外

因为显卡是由BIOS初始化的，内核无法获取必要的恢复信息(仅存在于BIOS中，内核无法读取)。

这个选项允许内核以两种方式尝试使用ACPI子系统来恢复显卡的状态。

>

	acpi_sci= [HW,ACPI] ACPI System Control Interrupt trigger mode

	Format: { level | edge | high | low }

ACPI系统控制终端触发器模式

	acpi_irq_balance [HW,ACPI]

使ACPI对中断请求进行平衡，在APIC模式下为默认值

	acpi_irq_nobalance [HW,ACPI]

ACPI不对中断请求进行平衡（默认），PIC模式下为默认值

	acpi_irq_pci= [HW,ACPI]

	Format: <irq>,<irq>...

如果启用了irq_balance则将列出的中断号标记为已经被PCI子系统使用，可用于屏蔽某些中断。

>

	acpi_irq_isa= [HW,ACPI]

	Format: <irq>,<irq>...

如果irq balance选项被开启，表示列出的irq号被isa子系统使用。

>

	acpi_os_name= [HW,ACPI]

告诉ACPI BIOS操作系统的名称。

常常用来哄骗某些老旧的BIOS以为运行的是Windows系统。

比如"Microsoft 2001"表示WinXP，"Microsoft Windows"表示Win98。

>

	acpi_osi= [HW,ACPI]

空参数是关闭 _OSI方法.

>

	acpi_serialize  [HW,ACPI]

强制串行化ACPI机器语言(ACPI Machine Language)方法，操作系统使用这种语言与BIOS打交道。

>

	acpi_skip_timer_override [HW,ACPI]

对于某些有毛病的 Nvidia NF5 主板需要使用此选项才能正常使用，不过此时 HPET 将失效。

>

	acpi_dbg_layer= [HW,ACPI]

	acpi_dbg_level= [HW,ACPI]

每一个int位标明一个debug layer或level，1开启，0关闭。

对于系统启动时debug acpi有用。

启动后可以通过/proc/acpi/debug_layer 或 debug_level 设置。

>

	acpi_fake_ecdt  [HW,ACPI]

当bios缺乏 ecdt，允许acpi去代替它工作。

>

Embedded Controller Boot Resources Table (ECDT)

This optional table provides the processor-relative, translated resources of an Embedded Controller. The

presence of this table allows OSPM to provide Embedded Controller operation region space access before

the namespace has been evaluated. If this table is not provided, the Embedded Controller region space will

not be available until the Embedded Controller device in the AML namespace has been discovered and

enumerated. The availability of the region space can be detected by providing a _REG method object

underneath the Embedded Controller device.

>

	acpi_generic_hotkey   [HW,ACPI]

允许acpi使用通用热插拔驱动去代替平台特殊的驱动。

>

	acpi_pm_good    [IA-32,X86-64]

跳过pmtimer的bug检测，强制内核假设这台机器的pmtimer没有毛病。

用于解决某些有缺陷的BIOS。

>

	memmap=nn[KMG]#ss[KMG]   [KNL,ACPI]

将从ss开始的nn长度的内存区域标记为ACPI数据。

>

	memmap=nn[KMG]$ss[KMG]   [KNL,ACPI]

将从ss开始的nn长度的内存区域标记为"保留"。

>

	pnpacpi=off  [ACPI]

禁用ACPI的即插即用功能，而使用PNPBIOS来代替。

>

	processor.max_cstate=  [HW,ACPI]

	{0|1|2|3|4|5|6|7|8|9}

无视ACPI表报告的值，强制制定CPU的最大C-state值。

这里的数字必须是一个有效的C-state值，

比如Yonah处理器支持"0-4"五个级别：C0为正常状态，其他则为不同的省电模式(数字越大表示CPU休眠的程度越深/越省电)。

"9"表示超越所有的DMI黑名单限制。

你的CPU的 95%的时间应该处于最深度的idle状态。

>


	processor.nocst

不使用_CST方法来侦测C-state值，而是使用传统的FADT方法。

>


	ec_intr=n

指定acpi中断控制器的模式。

如果n是0那么轮询模式将被启用，否则中断模式将被使用，默认是中断模式。 

>

[/acpi]
