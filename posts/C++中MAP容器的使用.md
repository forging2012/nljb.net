---
title: C++中MAP容器的使用
date: '2014-09-28'
description:
categories:

tags:c++

---

MAP的说明  

	1, 头文件 
	#include <map> 

	2, 定义 
	map<string, int> my_Map; 或者是typedef map<string, int> MY_MAP; 
	MY_MAP my_Map; 

	3, 插入数据 
	(1) my_Map["a"] = 1; 
	(2) my_Map.insert(map<string, int>::value_type("b",2)); 
	(3) my_Map.insert(pair<string,int>("c",3)); 
	(4) my_Map.insert(make_pair<string,int>("d",4)); 

	4, 查找数据和修改数据 
	(1) int i = my_Map["a"]; 
	    my_Map["a"] = i; 
	(2) MY_MAP::iterator my_Itr; 
	    my_Itr.find("b"); 
	    int j = my_Itr->second; 
	    my_Itr->second = j; 
	不过注意，键本身是不能被修改的，除非删除。 

	5, 删除数据 
	(1) my_Map.erase(my_Itr); 
	(2) my_Map.erase("c"); 
	还是注意，第一种情况在迭代期间是不能被删除的，道理和foreach时不能删除元素一样。 

	6, 迭代数据 
	for (my_Itr=my_Map.begin(); my_Itr!=my_Map.end(); ++my_Itr) {} 

	7, 其它方法 
	my_Map.size() 返回元素数目 
	my_Map.empty() 判断是否为空 
	my_Map.clear() 清空所有元素 
	可以直接进行赋值和比较：=,   >,   >=,   <,   <=,   !=   等等 

---

	// 判断KEY是否存在
	bool Exist(const keyName)
	{
	     return (mRegistryMap.find(keyName) != mRegistryMap.end());
	}

---

	#ifndef INI_H
	#define INI_H

	#include <map>
	#include <string>

	using namespace std;

	typedef map<string, string> values;
	typedef map<string, values*> keys;

	class INI
	{
	public:
	    INI(string);
	    ~INI();
	public:
	    int ReadINI();
	    int WriteINI();
	    void ClearINI();
	    void ShowINI();
	    void AppendValByKeysAndVals(string, string, string);
	    int DelValByKeysAndVals(string, string);
	    string GetValByKeysAndVals(string, string);
	private:
	    keys * key;
	    string path;
	};

	#endif // INI_H

---

	#include "ini.h"
	#include <fstream>
	#include <string>
	#include <iostream>
	#include <string.h>

	INI::INI(string _path)
	{
	    key = new keys();
	    path = _path;
	}

	INI::~INI()
	{
	    ClearINI();
	    delete(key);
	}

	int INI::ReadINI()
	{
	    ClearINI();
	    string line;
	    string type;
	    int i;
	    ifstream f(path.c_str());
	    if (!f.is_open()){
		return -1;
	    }
	    while(getline(f, line)){
		if (line.substr(0,1) ==  "#" || line.empty())
		{
		    continue;
		}
		if (line.substr(0,1) ==  "[")
		{
		    type = line.substr(1,line.length()-2);
		    continue;
		}
		if(type.empty()) {
		    continue;
		}
		i = line.find("=");
		if(i == -1)
		{
		    continue;
		}
		if(key->find(type) == key->end())
		{
		    values * val = new values();
		    (*key)[type] = val;
		}
		values * val = (*key)[type];
		(*val)[line.substr(0,i)] = line.substr(line.find("=")+1,line.length());
	    }
	    f.close();
	    return 0;
	}

	void INI::ClearINI()
	{
	    keys::iterator it;
	    for(it=(*key).begin(); it!=(*key).end(); it++)
	    {
		(*(*it).second).clear();
		delete((*it).second);
		(*key).erase((*it).first);
	    }
	}

	void INI::ShowINI()
	{
	    keys::iterator it;
	    for(it=(*key).begin(); it!=(*key).end(); it++)
	    {
		cout << "[" << (*it).first << "]" << endl;
		values * vals = (*it).second;
		values::iterator iv;
		for(iv=(*vals).begin(); iv!=(*vals).end(); iv++)
		{
		    cout << (*iv).first << "=" << (*iv).second << endl;
		}
	    }
	}

	string INI::GetValByKeysAndVals(string _keys, string _values)
	{
	    if (key->find(_keys) == key->end())
	    {
		return "";
	    }
	    values * vals = (*key)[_keys];
	    if (vals->find(_values) == vals->end())
	    {
		return "";
	    }
	    return (*vals)[_values];
	}

	int INI::DelValByKeysAndVals(string _keys, string _values)
	{
	    if (key->find(_keys) == key->end())
	    {
		return -1;
	    }
	    values * vals = (*key)[_keys];
	    if (vals->find(_values) == vals->end())
	    {
		return -1;
	    }
	    (*vals).erase(_values);
	    if ((*vals).size() == 0)
	    {
		delete(vals);
		(*key).erase(_keys);
	    }
	    return 0;
	}

	void INI::AppendValByKeysAndVals(string _keys, string _values, string _value)
	{
	    if(key->find(_keys) == key->end())
	    {
		values * val = new values();
		(*key)[_keys] = val;
	    }
	    values * val = (*key)[_keys];
	    (*val)[_values] = _value;
	}

	int INI::WriteINI()
	{
	    string data;
	    ofstream f(path.c_str());
	    if(!f.is_open())
	    {
		return -1;
	    }
	    keys::iterator it;
	    for(it=(*key).begin(); it!=(*key).end(); it++)
	    {
		data.append("[");
		data.append((*it).first);
		data.append("]");
		data.append("\n");
		values * val = (*it).second;
		values::iterator iv;
		for(iv=(*val).begin(); iv!=(*val).end(); iv++)
		{
		    data.append((*iv).first);
		    data.append("=");
		    data.append((*iv).second);
		    data.append("\n");
		}
	    }
	    f.write(data.c_str(), strlen(data.c_str()));
	    f.flush();
	    f.close();
	    return 0;
	}

