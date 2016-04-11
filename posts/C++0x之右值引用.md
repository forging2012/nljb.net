---
title: C++0x之右值引用
date: '2014-10-16'
description:
categories:

tags:c++

---

右值引用（及其支持的Move语意和完美转发）

是C++0x将要加入的最重大语言特性之一

这点从该特性的提案在C++ - State of the Evolution列表上高居榜首也可以看得出来。

从实践角度讲，它能够完美解决C++中长久以来为人所诟病的临时对象效率问题。

从语言本身讲，它健全了C++中的引用类型在左值右值方面的缺陷。

从库设计者的角度讲，它给库设计者又带来了一把利器。

从库使用者的角度讲，不动一兵一卒便可以获得“免费的”效率提升…

---

	std::vector<int> v = readFile();
	 
readFile()的定义是这样的：
	 
	std::vector<int> readFile()
	{
	  std::vector<int> retv;
	  … // fill retv
	  return retv;
	}
 
这段代码低效的地方在于那个返回的临时对象。

一整个vector得被拷贝一遍，仅仅是为了传递其中的一组int，当v被构造完毕之后，这个临时对象便烟消云散。

这完全是公然的浪费！

---

简而言之，问题可以描述为：
 
C++没有区分copy和move语意。
 
什么是move语意？记得auto_ptr吗？auto_ptr在“拷贝”的时候其实并非严格意义上的拷贝。

“拷贝”是要保留源对象不变，并基于它复制出一个新的对象出来。

但auto_ptr的“拷贝”却会将源对象“掏空”，只留一个空壳——一次资源所有权的转移。

---
 
这就是move。
 
Move语意的作用——效率优化

最初的例子——完美解决方案

在先前的那个例子中
 
	vector<int> v = readFile();
 
有了move语意的话，readFile就可以简单的改成：
 
	std::vector<int> readFile()
	{
	std::vector<int> retv;
	… // fill retv
	return std::move(retv); // move retv out
	}
	 
std::move就可以把retv掏空，即搬移出去，而搬家的最终目的地是v。

这样的话，从内存分配的角度讲，只有retv中进行的内存分配，在从retv到返回的临时对象

再从后者到目的地v的“move”过程中, 没有任何的内存分配（我是指vector内的缓冲区分配）

取而代之的是，先是retv内的缓冲区被“转移”到返回值临时对象中，然后再从临时对象中转移到v中。

