---
title: String转uint64_t
date: '2014-10-28'
description:
categories:

tags:c++

---

	#include <stdio.h>
	#include <stdlib.h>
	#include <iostream>
	#include <sstream>
	using namespace std;

	int main(int argc, char **argv)
	{
		if (argc < 2)
		{
			cerr<<"Usage: transfernum num"<<endl;
			return -1;
		}

		cout<<"Input Num:"<<argv[1]<<endl;

		unsigned long long ullNum1 = 0, ullNum2=0, ullNum3=0, ullNum4=0;
		ullNum1 = atoi(argv[1]);
		ullNum2 = atol(argv[1]);
		ullNum3 = atoll(argv[1]);
		ullNum4 = strtoull(argv[1], NULL, 10);

		cout<<"transfer by atoi: "<<ullNum1<<endl;
		cout<<"transfer by atol: "<<ullNum2<<endl;
		cout<<"transfer by atoll: "<<ullNum3<<endl;
		cout<<"transfer by strtoull: "<<ullNum4<<endl;

		// 这个方法是在网上找的，这里测试下是否管用
		stringstream strValue;
		strValue<<argv[1];
		unsigned long long value;
		strValue << value;
		cout<<value<<endl;

		return 0;
	}

	g++ main.cpp -o transfernum
	./transfernum 18446744073709551615  (这个值是2^64-1，即64位的最大值)

	输出：
	Input Num:18446744073709551615      
	transfer by atoi: 18446744073709551615     (atoi也正确了？奇怪)
	transfer by atol: 9223372036854775807
	transfer by atoll: 9223372036854775807
	transfer by strtoull: 18446744073709551615  
	4196992 使用网上找的方法发现不正确）

