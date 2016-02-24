---
title: Swift语言基础教程笔记
date: '2016-02-24'
description:
categories:

tags:ios

---

>

### Swift 数据类型

>

	整数类型 Int, UInt

	浮点类型 Float, Double 

	布尔类型 Bool

	字符类型 String
	
	字典类型 Dictionary

	数组类型 Array

	可选类型 optionals
		来处理值可能缺失的情况, 可选类型表示有值或没有值

	类型别名 typealias
		typealias Feet = Int

	类型安全 type safe

	类型推断 type inference

	强制解析 forced unwrapping
		当你确定可选类型确实包含值后，可以在可选的名字后加一个感叹号(!)来获取值

>

---

>

### 变量

>

	// 变量 (指定类型)
	var n:Int ; n = 1 ; print(n)

	// 变量 (未指定类型, 类型由初始化值决定)
	var nn = 1 ; nn = 2 ; print(nn)

>

---

>

### 常量

>

	// 常量
	let x = 100; print(x)
	let xx:Int = 100 ; print(xx)

>

---

>

### 字符串

>

	// 空字符串
	var stringA = ""
	stringA.isEmpty

	// 字符串长度
	var varA  = "xxx"
	print("\(varA.characters.count)")

>

	// 字符串连接
	var str = "abc"
	// 字符串与字符串
	str = str + "def"
	// 字符串与Int类型值
	str = "\(str)=\(100)"

>

	// 常用方法

	isEmpty 
		判断字符串是否为空，返回布尔值
	hasPrefix(prefix: String) 
		检查字符串是否拥有特定前缀
	hasSuffix(suffix: String) 
		检查字符串是否拥有特定后缀。
	Int(String) 
		转换字符串数字为整型
		let myString: String = "256"
		let myInt: Int? = Int(myString)
	String.characters.count 
		计算字符串的长度
	utf8 
		可以通过遍历String的utf8属性来访问它的UTF-8编码
	utf16 
		可以通过遍历String的utf16属性来访问它的UTF-16编码
	unicodeScalars 
		可以通过遍历String值的unicodeScalars属性来访问它的Unicode标量编码.
	+
		连接两个字符串，并返回一个新的字符串
	+=
		连接操作符两边的字符串并将新字符串赋值给左边的操作符变量
	==
		判断两个字符串是否相等
	< 
		比较两个字符串，对两个字符串的字母逐一比较。
	!=
		比较两个字符是否不相等。

>

---

>

### 运算符

>

	Exp1 ? Exp2 : Exp3;

>

---

>

### 数组

>

	// 我们可以使用构造语法来创建一个由特定数据类型构成的空数组：
	var someArray = [SomeType]()

	// 以下是创建一个初始化大小数组的语法：
	var someArray = [SomeType](count: NumbeOfElements, repeatedValue: InitialValue)

	// 以下实例创建了一个类型为 Int ，大小为 3，初始值为 0 的空数组：
	var someInts = [Int](count: 3, repeatedValue: 0)

	// 以下实例创建了含有三个元素的数组：
	var someInts:[Int] = [10, 20, 30]

	// 我们可以根据数组的索引来访问数组的元素，语法如下：
	var someVar = someArray[index]

>

	// 数组
	var arr1 = []
	var arr2 = ["abc", 123]
	var arr3 = [String]()
	var arr4:[String] = ["abc", "bcd"]

	// 遍历
	for item in someStrs {
		print(item)
	}

	// 合并
	var intsA = [Int](count:2, repeatedValue: 2)
	var intsB = [Int](count:3, repeatedValue: 1)
	var intsC = intsA + intsB

	// isEmpty
	intsC.isEnpty

>

---

>
