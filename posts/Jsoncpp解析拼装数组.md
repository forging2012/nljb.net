---
title: Jsoncpp解析拼装数组
date: '2014-10-29'
description:
categories:

tags:c++

---

	// 例子一:
	string strValue =
		 "{\"ldh\":\"001\",\"gfc\":\"002\",\"yyj\":\"003\",\"andy\":[\"005\",\"123\",\"true\"]}";
	Json::Reader read;
	Json::Value value;
	value["ldh"] = "001";
	value["gfc"] = "002";
	value["andy"].append( "005" );
	value["andy"].append( "123" );
	value["andy"].append( "true" );
	if( read.parse( strValue,value ) )
	{
		Json::Value val_array = value["andy"];
		int iSize = val_array.size();
		for ( int nIndex = 0;nIndex < iSize;++ nIndex )
		{
			cout << val_array[nIndex] << endl;
		}
	}

---

	// 例子二:
	Json::Reader read;
	Json::Value value;
	value["ldh"] = "001";
	value["gfc"] = "002";
	Value item;
	Value array;
	item["andy1"] = "005";
	array.append( item );
	item["andy1"] = "123";
	array.append( item );
	item["andy1"] = "true";
	array.append( item );
	value["andy"] = array;
	cout << value.toStyledString() << endl;
	Json::Value val_array = value["andy"];
	int iSize = val_array.size();
	for ( int nIndex = 0;nIndex < iSize;++ nIndex )
	{
		cout << val_array[nIndex] << endl;
		if ( !val_array[nIndex]["andy1"].isNull() )
		{
			cout << val_array[nIndex]["andy1"] << endl;
		}
	}

---

	// 例子三:
	std::string strValue =
		 "{\"name\":\"json\",\"array\":[{\"cpp\":\"jsoncpp\"},{\"java\":\"jsoninjava\"}]}";
	Json::Value value;
	Reader read;
	if ( !read.parse( strValue,value ) )
	{
		return -1;
	}
	cout << value.toStyledString() << endl;
	Json::Value val_array = value["array"];
	int iSize = val_array.size();
	for ( int nIndex = 0;nIndex < iSize;++ nIndex )
	{
		cout << val_array[nIndex] << endl;
		if ( val_array[nIndex].isMember( "cpp" ) )
		{
			cout << val_array[nIndex]["cpp"] << endl;
		}
	}
