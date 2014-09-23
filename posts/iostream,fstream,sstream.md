---
title: iostream,fstream,sstream
date: '2014-09-23'
description:
categories:

tags:c++

---

<img src="{{urls.media}}/iostream,fstream,sstream/iostream.gif" alt="" width="600">

---

<img src="{{urls.media}}/iostream,fstream,sstream/2.jpg" alt="" width="600">

---
文件的输入和输出:

	ifstream：由istream派生而来，提供读文件的功能
	ofstream：由ostream派生而来，提供写文件的功能
	fstream：有iostream派生而来，提供读写同一个文件的功能

---
字符串流:

	istringstream：由istream派生而来，提供读string的功能
	ostringstream：由ostream派生而来，提供写string的功能
	stringstream：由iostream派生而来，提供读写string的功能

---
文件流:

	ifstream infile;

	// ifstream infile(path)
	infile.open(path.c_str()); 

	// 如果没有打开成功文件
	if(!infile)

	ofstream outfile

	// 第二个参数默认为out，清空文件准备写。app为在文件问追加。
	outfile.open(path.c_str(), ofstream::app); 

	// 最好在打开一文件前
	// 关闭可能张打开的文件，和清空错误标志；   
	in.close(); in.clear(); 

---
字符串流:

	string line,word;
	while(getline(cin, line))
	{

	    istringstream string_stream(line);
	    while(string_stream >> word)
		cout<<word<<endl;

	}

---
流之间可以相互转换:

	stringstream word_stream("countryhu!");
	cout << word_stream.str()<<endl;
	word_stream.str("hi,countryhu!");

	// 流读取一个字符
	stream.get()

---
例子:

	ifstream inf("/json");
	stringstream in ;
	in << inf.rdbuf();
	in.str().c_str();
	inf.close();


