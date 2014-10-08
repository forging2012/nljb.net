---
title: Boost-组件库编译方法
date: '2014-10-08'
description:
categories:

tags:c++

---

Boost库是一个可移植、提供源代码的C++库，作为标准库的后备，是C++标准化进程的开发引擎之一。

Boost库由C++标准委员会库工作组成员发起，其中有些内容有望成为下一代C++标准库内容。

在C++社区中影响甚大，是不折不扣的“准”标准库。

Boost由于其对跨平台的强调，对标准C++的强调，与编写平台无关。

大部分boost库功能的使用只需包括相应头文件即可。

但Boost中也有很多是实验性质的东西，在实际的开发中实用需要谨慎。

---

	// 执行(得到b2)
	./bootstrap.sh --prefix=/usr/local

---

	// 编译所有
	./b2 install --with=all

---

	// 指定编译
	./b2 install --with=thread

	    - atomic                   : not building
	    - chrono                   : not building
	    - context                  : not building
	    - date_time                : not building
	    - exception                : not building
	    - filesystem               : not building
	    - graph                    : not building
	    - graph_parallel           : not building
	    - iostreams                : not building
	    - locale                   : not building
	    - math                     : not building
	    - mpi                      : not building
	    - program_options          : not building
	    - python                   : not building
	    - random                   : not building
	    - regex                    : not building
	    - serialization            : not building
	    - signals                  : not building
	    - system                   : not building
	    - test                     : not building
	    - thread                   : building
	    - timer                    : not building
	    - wave                     : not building

