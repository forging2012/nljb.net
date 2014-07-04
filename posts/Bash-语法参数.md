---
title: Bash-语法参数
date: '2014-07-04'
description:
categories:

tags:system

---

	[ -a FILE ] 如果 FILE 存在则为真。
	[ -b FILE ] 如果 FILE 存在且是一个块特殊文件则为真。
	[ -c FILE ] 如果 FILE 存在且是一个字特殊文件则为真。
	[ -d FILE ] 如果 FILE 存在且是一个目录则为真。
	[ -e FILE ] 如果 FILE 存在则为真。
	[ -f FILE ] 如果 FILE 存在且是一个普通文件则为真。
	[ -g FILE ] 如果 FILE 存在且已经设置了SGID则为真。
	[ -h FILE ] 如果 FILE 存在且是一个符号连接则为真。
	[ -k FILE ] 如果 FILE 存在且已经设置了粘制位则为真。
	[ -p FILE ] 如果 FILE 存在且是一个名字管道(F如果O)则为真。
	[ -r FILE ] 如果 FILE 存在且是可读的则为真。
	[ -s FILE ] 如果 FILE 存在且大小不为0则为真。
	[ -t FD ] 如果文件描述符 FD 打开且指向一个终端则为真。
	[ -u FILE ] 如果 FILE 存在且设置了SUID (set user ID)则为真。
	[ -w FILE ] 如果 FILE 如果 FILE 存在且是可写的则为真。
	[ -x FILE ] 如果 FILE 存在且是可执行的则为真。
	[ -O FILE ] 如果 FILE 存在且属有效用户ID则为真。
	[ -G FILE ] 如果 FILE 存在且属有效用户组则为真。
	[ -L FILE ] 如果 FILE 存在且是一个符号连接则为真。
	[ -N FILE ] 如果 FILE 存在 and has been mod如果ied since it was last read则为真。
	[ -S FILE ] 如果 FILE 存在且是一个套接字则为真。
	[ FILE1 -nt FILE2 ] 如果 FILE1 has been changed more recently than FILE2, or 如果 FILE1FILE2 does not则为真。
	exists and [ FILE1 -ot FILE2 ] 如果 FILE1 比 FILE2 要老, 或者 FILE2 存在且 FILE1 不存在则为真。
	[ FILE1 -ef FILE2 ] 如果 FILE1 和 FILE2 指向相同的设备和节点号则为真。
	[ -o OPTIONNAME ] 如果 shell选项 “OPTIONNAME” 开启则为真。
	[ -z STRING ] “STRING” 的长度为零则为真。
	[ -n STRING ] or [ STRING ] “STRING” 的长度为非零 non-zero则为真。
	[ STRING1 == STRING2 ] 如果2个字符串相同。 “=” may be used instead of “==” for strict POSIX compliance则为真。
	[ STRING1 != STRING2 ] 如果字符串不相等则为真。
	[ STRING1 < STRING2 ] 如果 “STRING1” sorts before “STRING2” lexicographically in the current locale则为真。
	[ STRING1 > STRING2 ] 如果 “STRING1” sorts after “STRING2” lexicographically in the current locale则为真。
	[ ARG1 OP ARG2 ] “OP” is one of -eq, -ne, -lt, -le, -gt or -ge. These arithmetic binary operators return true if “ARG1” is equal to, not equal to, less than, less than or equal to, greater than, or greater than or equal to “ARG2”, respectively. “ARG1” and “ARG2” are integers. 表达式可以借以下操作符组合起来，以降序列出：listed in decreasing order of precedence: 表 7.2. 组合表达式操作 效果
	[ ! EXPR ] 如果 EXPR 是false则为真。
	[ ( EXPR ) ] 返回 EXPR的值。这样可以用来忽略正常的操作符优先级。
	[ EXPR1 -a EXPR2 ] 如果 EXPR1 and EXPR2 全真则为真。
	[ EXPR1 -o EXPR2 ] 如果 EXPR1 或者 EXPR2 为真则为真。

---

比较字符写法：

	-eq 等于
	-ne 不等于
	-gt 大于
	-lt 小于
	-le 小于等于
	-ge 大于等于
	-z 空串
	* = 两个字符相等
	* != 两个字符不等
	* -n 非空串

	从大到小排序是:ls -S  /
	从小到大排就加个-r:  ls -Sr  /
	包括隐藏文件就是：ls -aSr  /
	ls -l 是长格式显示
	ls -a 是包含隐藏文件
	所以一般使用ls -la 

	不可以，sort是处理数据流的，一般以行为单位，把每行按某种条件排列
	如字母顺序、数字大小
	它可以排列从文件内读取的数据
	ls | sort 这个命令排列的也仅是文件名而已
	ls -tr
	-r 对目录反向排序。

