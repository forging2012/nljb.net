---
title: C++中使用boost::property_tree解析INI
date: '2014-11-03'
description:
categories:

tags:c++

---

	#ifndef INICONFIG_H
	#define INICONFIG_H

	#include <boost/property_tree/ptree.hpp>
	#include <boost/property_tree/ini_parser.hpp>
	#include <string>
	#include <vector>

	using namespace std;
	using namespace boost;
	using namespace boost::property_tree;

	class INIConfig
	{
	public:
	    INIConfig(string);
	    ~INIConfig();
	public:
	    bool ReadINI();
	    bool WriteINI();
	    bool AddString(string, string);
	    bool PutString(string, string);
	    string GetString(string, string);
	    int GetInt(string, int);
	    vector<string> GetChildString(string);
	private:
	    string path;
	    ptree conf;
	};

	#endif // INICONFIG_H

---

	#include "include/INIConfig.h"
	#include <vector>
	#include <boost/format.hpp>

	INIConfig::INIConfig(string _path)
	{
	    path = _path;
	}

	INIConfig::~INIConfig(){}

	bool INIConfig::ReadINI()
	{
	    try
	    {
		read_ini(path, conf);
	    }
	    catch (std::exception& e)
	    {
		return false;
	    }
	    return true;
	}

	bool INIConfig::AddString(string _path, string _value)
	{
	    try
	    {
		conf.add(_path, _value);
	    }
	    catch (std::exception& e)
	    {
		return false;
	    }
	    return true;
	}

	bool INIConfig::PutString(string _path, string _value)
	{
	    try
	    {
		conf.put(_path, _value);
	    }
	    catch (std::exception& e)
	    {
		return false;
	    }
	    return true;
	}

	string INIConfig::GetString(string _path, string _default)
	{
	    return conf.get<string>(_path, _default);
	}

	int INIConfig::GetInt(string _path, int _default)
	{
	    return conf.get<int>(_path, _default);
	}

	vector<string> INIConfig::GetChildString(string _path)
	{
	    vector<string> child;
	    try
	    {
		auto x = conf.get_child(_path);
		for (auto& pos : x)
		{
		    child.push_back(pos.first);
		}
	    }
	    catch (std::exception&)
	    {
		return std::move(child);
	    }
	    return std::move(child);
	}

	bool INIConfig::WriteINI()
	{
	    try
	    {
		write_ini(path, conf);
	    }
	    catch (std::exception&)
	    {
		return false;
	    }
	    return true;
	}

