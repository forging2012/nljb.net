---
title: Linux-date-命令使用技巧
date: '2014-07-04'
description:
categories:

tags:system

---

	[root@localhost ~]# date --set "1/1/09 00:01" <== （月/日/年时:分:秒）
	[root@localhost ~]# date 012501012009.30 <== 月日时分年.秒
	Linux查看日期时间命令 date 把系统时间设置为以下时间： date -s 2010-04-07 date -s 10:20:33

------------------------------------------------------------------------------

date命令 

	　　date命令的功能是显示和设置系统日期和时间。 
	　　该命令的一般格式为： date [选项] 显示时间格式（以+开头，后面接格式） 
	　　date 设置时间格式 
	　　命令中各选项的含义分别为： 
	　　-d datestr, --date datestr 显示由datestr描述的日期 
	　　-s datestr, --set datestr 设置datestr 描述的日期 
	　　-u, --universal 显示或设置通用时间 

时间域 

	　　% H 小时（00..23） 
	　　% I 小时（01..12） 
	　　% k 小时（0..23） 
	　　% l 小时（1..12） 
	　　% M 分（00..59） 
	　　% p 显示出AM或PM 
	　　% r 时间（hh：mm：ss AM或PM），12小时 
	　　% s 从1970年1月1日00：00：00到目前经历的秒数 
	　　% S 秒（00..59） 
	　　% T 时间（24小时制）（hh:mm:ss） 
	　　% X 显示时间的格式（％H:％M:％S） 
	　　% Z 时区 日期域 
	　　% a 星期几的简称（ Sun..Sat） 
	　　% A 星期几的全称（ Sunday..Saturday） 
	　　% b 月的简称（Jan..Dec） 
	　　% B 月的全称（January..December） 
	　　% c 日期和时间（ Mon Nov 8 14：12：46 CST 1999） 
	　　% d 一个月的第几天（01..31） 
	　　% D 日期（mm／dd／yy） 
	　　% h 和%b选项相同 
	　　% j 一年的第几天（001..366） 
	　　% m 月（01..12） 
	　　% w 一个星期的第几天（0代表星期天） 
	　　% W 一年的第几个星期（00..53，星期一为第一天） 
	　　% x 显示日期的格式（mm/dd/yy） 
	　　% y 年的最后两个数字（ 1999则是99） 
	　　% Y 年（例如：1970，1996等） 
	　　需要特别说明的是，只有超级用户才能用date命令设置时间，一般用户只能用date命令显示时间。 
	　　例1：用指定的格式显示时间。 
	　　$ date ‘+This date now is =>%x ，time is now =>%X ，thank you !' 
	　　This date now is =>11/12/99 ，time is now =>17:53:01 ，thank you ! 
	　　例2：用预定的格式显示当前的时间。 
	　　# date 
	　　Fri Nov 26 15：20：18 CST 1999 
	　　例3：设置时间为下午14点36分。 
	　　# date -s 14:36:00 
	　　Fri Nov 26 14：15：00 CST 1999 
	　　例4：设置时间为1999年11月28号。 
	　　# date -s 991128 
	　　Sun Nov 28 00：00：00 CST 1999 
	    例5：设置一天前
	    date --date "1 days ago" +"%Y-%m-%d"
