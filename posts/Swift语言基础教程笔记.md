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
	字符类型 String, Character
	字典类型 Dictionary
	数组类型 Arraya(List), Tuples(元组)
	集合理性 Set(无序数据集合)
	函数类型 Function
	闭包类型 Closure
	可选类型 optionals
	类型别名 typealias
	类型安全 type safe
	类型推断 type inference
	强制解析 forced unwrapping

>

	let imInt:Int = 2
	let imDouble:Double = 3.1415926
	let imBool:Bool = true
	let imString:String = "Hello"
	let imTuple:(Int, Int) = (2, 4)
	let imPptional:Int? = nil
	let imCharacter:Character = "!"

>

---

>

### 可选类型

>

	// 当值可能不存在（may be absent）的时候使用Optionals。

	// 声明一个 Optionals 类型
	var optionalInteger: Int?
	var optionalInteger: Optional<Int>

	// 当你确定可选类型确实包含值之后
	// 你可以用后缀!来访问这个值
	optionalInteger = 42
	optionalInteger! // 42

	// 使用操作符！去获取值为nil的可选变量会有运行时错误
	
>
	
	// 普通的函数
	func sayHello(name:String, _ addr:String) {
	    print(name, addr)
	}
	
	// 传入两个参数（可选型）
	var name:String? = "nljb"
	var addr:String? = "beijing"
	
	// 需要解包才可以传入
	// 当然可以这样(name!, addr!)
	// 也可以使用if-let安全解包
	// 并且if-let添加判断条件
	if let name = name where name == "jbnl", let addr = addr {
	    sayHello(name, addr)
	} else {
	    print("no nljb")
	}

>

	// 如果值为nil，任何操作都不会执行

	var myString:String? = nil
	if myString != nil {
		print(myString)
	}else{
		print("nil")
	}
	// 输出 nil

>

	// 强制解析

	var myString:String?
	myString = "Hello, Swift!"
	if myString != nil {
	   print(myString)
	}else{
	   print("myString 值为 nil")
	}
	// 输出 Optional("Hello, Swift!")	

	var myString:String?
	myString = "Hello, Swift!"
	if myString != nil {
	   // 强制解析
	   print( myString! )
	}else{
	   print("myString 值为 nil")
	}
	// 输出 Hello, Swift!
	
>

	// 自动解析

	// 你可以在声明可选变量时使用感叹号（!）替换问号（?）。
	// 这样可选变量在使用时就不需要再加一个感叹号（!）来获取值，它会自动解析。

	var myString:String!
	myString = "Hello, Swift!"
	if myString != nil {
	   print(myString)
	}else{
	   print("myString 值为 nil")
	}
	// 输出 Hello, Swift!

>

	// 可选绑定

	// 使用可选绑定（optional binding）来判断可选类型是否包含值
	// 如果包含就把值赋给一个临时常量或者变量。
	
	// 可选绑定可以用在if和while语句中来对可选类型的值进行判断并把值赋给一个常量或者变量。

	var myString:String?
	myString = "Hello, Swift!"
	if let yourString = myString {
	   print("你的字符串值为 - \(yourString)")
	}else{
	   print("你的字符串没有值")
	}
	// 你的字符串值为 - Hello, Swift!

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

	import Foundation
	// 可以用String使用原来在OC语言里NSString中所有的方法

>

---

>

### 元运算

>

	// 三元
	Exp1 ? Exp2 : Exp3;

	// 二元
	a ?? b -> a != nil ? a! : b

	// a 必须为 Optionals 类型
	// b 类型必须是 a 解包后类型

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

### 元组

>

	// 元组（tuples）是把多个值组合成一个复合值
	// 元组内的值可以使任意类型

>

	let http404Error = (404, "Not Found")
	// 将一个 Int 类型值和一个 String 类型值组合在一起

	// 可以将一个元组的内容分解成单独的常量或变量：
	let (statusCode, statusMessage) = http404Error
	println("The status code is \(statusCode)")
	println("The status message is \(statusMessage)")

	// 如果你只需要一部分元组的值，忽略的部分用下划线(_)标记:
	let (justTheStatusCode, _) = http404Error
	println("The status code is \(justTheStatusCode)")

	// 另外，可以使用索引访问元组中的各个元素，索引数字从0开始:
	println("The status code is \(http404Error.0)")
	println("The status message is \(http404Error.1)")

	// 可以给元组的各个元素进行命名:
	let http200Status = (statusCode: 200, description: "OK")

	// 这时，可以使用元素名来访问这些元素的值:
	println("The status code is \(http200Status.statusCode)")
	println("The status message is \(http200Status.description)")

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

### 函数

>

---

>

	// 值类型
	值类型传入的都是对象的副本，对值类型的修改不会影响原对象
	Int Float Double Bool Tuple String Array Dictionarry
	
	// 引用类型
	引用类型传入的都是对象的引用，对引用类型的修改会影响原对象
	Function（函数） Closure(闭包）
	

>

---

>

	// Swift 定义函数使用关键字 func

	// Swift func 是可以进行嵌套使用的

	// 可以指定一个或多个输入参数和一个返回值类型
	// 无参函数 func runoob() -> String 
	// 单个参数 func runoob(site:String) -> String 
	func runoob(one:String, two:String) -> String {
		return site
	}
	print(runoob("xxx"))

	// 例
	func sayHello(name:String?) -> String {
		// 这里如果用户没有指定名称则返回Guest
		let result = "Hello, " + (name ?? "Guest") + "!"
		return result
	}
	var nickname:String?
	nickname = "World"
	print(sayHello(nickname))

>

	// 常量参数
	func sayHello(name:String) -> String {
		// 传值操作
		// 则此时name是常量参数不可以修改
	}

	// 变量参数
	func sayHello(var name:String) -> String {
		// 传值操作
		// 局部变量参数，修改不会影响外部
		// 则此时name是变量参数可以修改
	}

>

	// 通过元组返回多个值
	func maxminScores(scores:[Int]) -> (maxscore:Int, minscore:Int)	{
		...
		return (scores[0], scores[1])
	}
	
	// 元组返回异常情况, 则这里需要使用 optionals 可选型
	func maxminScores(scores:[Int]) -> (maxscore:Int, minscore:Int)? {
        if scores.isEmpty {
			return nil
		}
        return (scores[0], scores[1])
    }

>

	// 内部函数名
	// 外部函数名

	// userName 外部函数名, greetingWord 外部函数名
	// nickname 内部函数名, greetingWord 内部函数名
	func sayHello(userName nickname:String, greetingWord greeting:String) -> String {
		...
	}

	// 使用外部函数名
	sayHello(userNmae: "xxx", greetingWord: "xxx")	
	
	// nickname 既是内部参数名，又是外部参数名
	// greeting 既是内部参数名，又是外部参数名
	func sayHello(#nickname:String, #greeting:String) -> String {	
		...
	}

>

	// 参数的默认值
	func sayHello(nickname:String, greeting:String = "nljb" ) -> String {	
		...
	}

	// 使用时如果不传入greeting则使用默认值
	sayHello("xxx")

	// 如果设置了参数默认值则必须使用外部函数名
	// 如果不指定参数默认值的外部函数名则自动使用内部函数名
	sayHello("xxx", greeting: "xxx")

	// _ 取消苹果为参数默认值所指定强制外部函数名的设置
	func sayHello(nickname:String, _ greeting:String = "nljb" ) -> String {	
		...
	}

	// 通过外部函数名可以指定传入的参数
	func sayHello(nickname:String, greeting:String = "nljb", others:String = "com") -> String {	
		...
	}

	// ...
	sayHello("www", others: "net")

>

	// 传引用值, 当声明参数的时候加上 inout
	// 则如果修改该值，外部的值也随之改变
	func sayHello(inout a:Int, inout b:Int) {
		a = 100
		b = 100	
	}

	// ...
	var x = 0
	var y = 0
	sayHello(&x, b: &y)

>

	// 函数类型
	func sayHello(a:Int, b:Int) -> String {
		...
    }

	// 则 another 当 sayHello 用
	let another = sayHello

	// 指定类型
	let another:(Int, Int) -> String = sayHello

	// 返回值为空也必须写 () or Void
	let another:(Int, Int) -> () = sayHello

>

	// ...
	func sayWorld() -> Int {
		return 99;
	}

	// 返回函数类型
	func sayHello() () -> Int {
		return sayWorld();
	}

	let x = sayHello()
	x()

>

---

>

### 闭包

>

	// 接受闭包的函数
	func sayHello(a:Int, _ x:(a:Int, b:Int) -> Int) -> Int { return x(a: a,b: a) }
	
	// 传入闭包
	let x = sayHello(100, {(a:Int, b:Int) -> Int in return a + b })
	
	// 传入闭包 简写
	let y = sayHello(100, {a, b in return a + b })
	
	// 传入闭包 简写
	let z = sayHello(100, {a, b in a + b })
	
	// 传入闭包 简写 传入参数($0 ... $n)
	let v = sayHello(100, {$0 + $1})
	
	// 传入闭包 简写 运算符
	let n = sayHello(100, +)

>

	// 结尾闭包 ... 当闭包是最后一个参数时
	let m = sayHello(100) {
	    (a:Int, b:Int) -> Int in
	    return a + b
	}
	
>

	// 闭包内使用外部变量
	var num = 5
	let h = sayHello(100) {
	    (a:Int, b:Int) -> Int in
	    return a + b + 5
	}
	
>

---

>

### 枚举

>

	// 枚举类型
	enum GameEnding {
	    case Win
	    case Lose
	    case Draw
	}
	
>
	
	// 使用案例
	
	var yourScore:Int = 0
	var enemyScore:Int = 0
	
	var theGameEnding:GameEnding = .Win
	if yourScore > enemyScore { theGameEnding = .Win }
	if yourScore < enemyScore { theGameEnding = .Lose }
	if yourScore == enemyScore { theGameEnding = .Draw }
	
	switch theGameEnding {
		case .Win: print("Win")
		case .Lose: print("Lose")
		case .Draw: print("Draw")
	}
	
>

	// 枚举类型（挂接值为Int, 并且自动++)
	enum Month:Int {
	    case Jan = 1, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec
	}
	
	// 枚举变量
	let m:Month = .Jan
	
	// 枚举变量，挂接值
	print(m.rawValue)
	
	// 挂接值创建变量 
	// 使用?的原因是rawValue不可控
	// 如果rawValue在Month中没有则返回nil
	let o:Month? = Month(rawValue: 12)

>

	// 枚举类型（挂接值为String)
	enum Month:String {
	    case OK = "Is Ok"
	    case Error = "Is Error"
	}
	
	// 可以进行状态定义
	func isOK() -> Month {
	    return Month.Error
	}
	
	if isOK() == Month.OK {
	    print(Month.OK.rawValue)
	} else {
	    print(Month.Error.rawValue)
	}
	
>

	// 枚举类型
	enum BarCode {
	    // 包含四个Int的UPCA
	    case UPCA(Int, Int, Int, Int)
	    // 包含一个String的QRcode
	    case QRcode(String)
	}
	
	// 枚举变量（方法一）
	let codeA = BarCode.UPCA(100, 200, 300, 400)
	// 枚举变量（方法二）
	let codeB:BarCode = .QRcode("Hello QRcode")
	
	// 使用变量
	switch codeA {
	    case .UPCA(let a, let b, let c, let d):
	        print(a,b,c,d)
	    case .QRcode(let s):
	        print(s)
	}

>

---
