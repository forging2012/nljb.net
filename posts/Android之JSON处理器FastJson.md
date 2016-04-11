---
title: Android之JSON处理器FastJson
date: '2015-07-23'
description:
categories:

tags:android

---

>

### FastJson

>

fastjson 是一个性能很好的 Java 语言实现的 JSON 解析器和生成器，来自阿里巴巴的工程师开发

>

	主要特点：

	快速FAST (比其它任何基于Java的解析器和生成器更快，包括jackson）
	强大（支持普通JDK类包括任意Java Bean Class、Collection、Map、Date或enum）
	零依赖（没有依赖其它任何类库除了JDK）

>

	https://github.com/alibaba/fastjson

>

	import com.alibaba.fastjson.JSON;
	 
	Group group = new Group();
	group.setId(0L);
	group.setName("admin");
	 
	User guestUser = new User();
	guestUser.setId(2L);
	guestUser.setName("guest");
	 
	User rootUser = new User();
	rootUser.setId(3L);
	rootUser.setName("root");
	 
	group.getUsers().add(guestUser);
	group.getUsers().add(rootUser); 
	String jsonString = JSON.toJSONString(group); 
	System.out.println(jsonString);



