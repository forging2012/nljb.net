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

	类型别名 typealias

	类型安全 type safe

	类型推断 type inference

	强制解析 forced unwrapping

>

---

>

### 可选类型

>

	// 当值可能不存在（may be absent）的时候使用Optionals。

	// 这个字符串是完全由数字组成的
	let possibleNumber:String = "123"
	// 这里调用toInt()方法时，转换会成功, 得到整型值123
	let convertedNumber = Int(possibleNumber)
	// 这里输出了Optional(123)
	print(convertedNumber)
	
	// 这个字符串不是由数字组成的
	let originalString:String = "hello, world"
	// 这里调用toInt()方法时，转换会失败
	// 所以返回的是Optional类型，也就是Int？类型
	let resultNumber = Int(originalString)
	// 这里输出了一个nil说明转换失败了什么都没有
	print(resultNumber)

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

>

---

>

### 三目运算

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

### 字典

>

	// 我们可以使用以下语法来创建一个特定类型的空字典：
	var someDict =  [KeyType : ValueType]()

	// 以下是创建一个空字典，键的类型为 Int，值的类型为 String 的简单语法：
	var someDict = [Int : String]()

	// 以下为创建一个字典的实例：
	var someDict:[Int:String] = [1:"One", 2:"Two", 3:"Three"]

	// 我们可以根据字典的索引来访问数组的元素，语法如下：
	var someVar = someDict[key]

	// 我们可以使用 updateValue(forKey:) 增加或更新字典的内容。
	// 如果 key 不存在，则添加值，如果存在则修改 key 对应的值。
	someDict:[Int:String] = [1:"One", 2:"Two", 3:"Three"]
	var oldVal = someDict.updateValue("One 新的值", forKey: 1)

	// 我们可以使用 removeValueForKey() 方法来移除字典 key-value 对。
	// 如果 key 存在该方法返回移除的值，如果不存在返回 nil 。实例如下：
	var someDict:[Int:String] = [1:"One", 2:"Two", 3:"Three"]
	var removedValue = someDict.removeValueForKey(2)

	// 我们可以使用 for-in 循环来遍历某个字典中的键值对。实例如下:
	var someDict:[Int:String] = [1:"One", 2:"Two", 3:"Three"]
	for (key, value) in someDict {
	   print("字典 key \(key) -  字典 value \(value)")
	}

	// 你可以提取字典的键值(key-value)对，并转换为独立的数组。实例如下：
	var someDict:[Int:String] = [1:"One", 2:"Two", 3:"Three"]
	let dictKeys = [Int](someDict.keys)
	let dictValues = [String](someDict.values)

	// 我们可以使用只读的 count 属性来计算字典有多少个键值对：
	var someDict1:[Int:String] = [1:"One", 2:"Two", 3:"Three"]
	var someDict2:[Int:String] = [4:"Four", 5:"Five"]
	print("someDict1 含有 \(someDict1.count) 个键值对")
	print("someDict2 含有 \(someDict2.count) 个键值对")

	// 我们可以通过只读属性 isEmpty 来判断字典是否为空，返回布尔值:	
	var someDict3:[Int:String] = [Int:String]()
	print("someDict1 = \(someDict1.isEmpty)")

>

---

>

---
