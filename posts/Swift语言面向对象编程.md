---
title: Swift语言面向对象编程
date: '2016-02-26'
description:
categories:

tags:ios

---

>

### 断言

>

*可以使用全局函数assert来声明一个断言。向assert函数传递一个条件表达式，如果表达式为false,可以打印一段信息*

	assert(1 > 2, "Error)

>

---

>

### Class

>

***注意：Class 是引用类型***

>

* Class 继承 扩展类型
* Class 构造 重载 静态方法 重写 父类方法

>

***注意：类和结构体的选择***

*结构体实例总是通过值传递，类实例总是通过引用传递，这说明两者适用于不同的任务，当在考虑一个工程项目的数据结构和功能的时候，你需要决定每个数据构造是定义类还是结构体*

>
	
***Class***

	// Class
	class Hi {
	    
	    var name:String
	    
	    var addr:String?
	    
	    // 构造方法
	    init(name:String) {
	        // This 指针 self
	        self.name = name
	    }
	    
	    // 构造重载
	    init(name:String, addr:String) {
	        self.name = name
	        self.addr = addr
	    }
	    
	    // 方法
	    func sayHi() {
	        if let x = addr {
	            print("Hi \(name) \(x)")
	        } else {
	            print("Hi \(self.name)")
	        }
	    }
	    
	    // 方法重载
	    func sayHi(name:String) {
	        print("Hi \(name)")
	    }
	    
	    // Class Func 静态方法
	    class func sayClass() {
	        print("Hi Class")
	    }
	    
	}

>

***Class 实例***

	// Class
	var hi = Hi(name: "nljb", addr: "beijing")
	hi.sayHi()
	
>

***Class 静态方法***

	// Class Func
	Hi.sayClass()

>
	
***Class 继承 与 重写***

	class Hello:Hi {
	    
	    // 重写方法
	    override func sayHi() {
	        // 执行父类方法
	        super.sayHi("Hello")
	        // ...
	       print("Hello \(name)")
	    }
	    
	}
	
	// Class
	var he = Hello(name: "jbnl")
	he.sayHi()
	
>
	
***Class 扩展***

	// 在原有类的基础上进行扩展
	extension Hi {
	    
	    // 扩展方法
	    func sayExten() {
	        print("Hi Exten")
	    }
	    
	}
	
	// Class
	var ex = Hi(name: "nljb", addr: "beijing")
	ex.sayExten()

>

---

>

### 接口

>

***Class protocol***

	// 接口
	protocol Pro {
	    // 接口方法
	    func sayHello() -> String
	}
	
***Class 继承接口***

	// 继承 Pro
	class Hi:Pro {
	    // 接口方法
	    func sayHello() -> String {
	        return "Hello"
	    }
	    
	}

>

---

>

### 标识恒等

>

***判定两个常量或者变量是否引用同一个类实例***

* 等价于（ === ）
* 不等价于（ !== ）

>

	class Hi { ... }
	var a = Hi() // 实例 a
	var b = Hi() // 实例 b
	var c = a // 实例 c 引用 a
	a === b // false
	a === c // true

>

---

>

### 属性

>

* 属性将值跟特定的类、结构体、枚举关联
* 存储属性存储常量或变量作为实例的一部分
* 计算属性计算一个值（而不是存储一个值）

>

* 计算属性可以用于类、结构体、枚举中
* 存储属性只能用于类和结构体，枚举不可以使用

>

***存储属性***

*一个存储数据就是存储在特定类或结构体实例中的一个常量(let)或变量(var)*

>

***注意：常量存储属性在创建赋值后无法修改它的值***

	// 结构体（值类型）
	struct Hello {
		// 存储属性（变量）
		var addr:Int
		// 存储属性（常量）
		let name:Int
	}
	
	// 结构体 变量实例
	var a = Hello(addr:0, name:0)
	
	// 此时变量存储属性是可以修改的
	a.addr = 1 // OK
	
	// 此时常量存储属性是不可以修改的
	a.name = 1 // Error
	
>

***注意：当值类型的实例被声明为常量的时候，它的所有属性也就成了常量***

	// 结构体 常量实例
	let b = Hello(addr:0, name:0)
	
	// 此时变量存储属性是不可修改的
	a.addr = 1 // Error
	
	// 此时常量存储属性是不可以修改的
	a.name = 1 // Error
	
	
***注意：引用类型的类则不一样，把一个引用类型的实例赋给一个常量后，仍然可以修改变量的属性***

	// Class（引用类型）
	class Hello {
	    var addr:Int = 0
	    let name:Int = 0
	}
	
	// 类（变量）
	var a = Hello()
	a.addr = 1 // OK
	a.name = 1 // Error
	
	// 类（常量）
	let b = Hello()
	b.addr = 1 // OK
	b.name = 1 // Error

>

***延迟存储属性***

*延迟存储属性是指当第一次被调用的时候才会计算其初始值的属性*

*必须将延迟属性声明成变量(var)*

*在属性声明前使用lazy来标识一个延迟存储属性*

*常量属性在构造完成前必须要有初始值，因此无法声明成延迟属性*

>

	class Init {
	    init(){
	        print("init ...")
	    }
	}
	
	class Hello {
		var i = Init()
	}
	
	var a = Hello()
	
	// 输出了 init ...
	
	// 把 var i = Init() 
	// 换 lazy var i = Init()
	// 没有输出 init ...
	
>
	
	class Init {
	    init(){
	        print("init ...")
	    }
	}
	
	class Hello {
		lazy var i = Init()
	}
	
	var a = Hello()
	a.i
	// 输出了 init ...

>

***计算属性***

*计算属性和属性监察器所描述的模式可以用于全局变量和本地变量*

*类、结构体、枚举可以定义计算属性*

*计算属性不直接存储值，而是提供一个getter来获取值*

*一个可选的setter来间接设置其它属性或变量的值*

>

	// 结构体
	struct Hello {
	    var x:Int
	    var y:Int
	}
	
	// 类
	class Hi {
	    
	    var h:Hello?
	    
	    // 计算属性
	    var a:Hello {
	        // 读取 a 时调用 Get
	        get {
	            print("run get ...")
	            return h ?? Hello(x: 0, y: 0)
	        }
	        // 设置 a 时调用 Set
	        // 也可以 set { ... } 则默认使用 newValue
	        set(hello) {
	            print("run set ...")
	            self.h = hello
	        }
	    }
	    
	}
	
	// 实例
	var h = Hi()
	
	// 设置 a 时调用 Set
	h.a = Hello(x: 5, y: 5)
	
	// 读取 a 时调用 Get
	print(h.a.x)
	print(h.a.y)
	
>

***只读计算属性***

*只有Get没有Set的计算属性就是只读计算属性*

*只读计算属性的声明可以去掉Get关键字和花括号*
	
	var a:Hello {
		return Hello(x: 0, y: 0)
	}

>

***属性监视器***

>

*计算属性和属性监察器所描述的模式可以用于全局变量和本地变量*

*属性监视器监控和响应属性值的变化*

*每次属性被设置的时候都会调用属性监视器，甚至新的值和现在的值相同的时候也不例外*

* willSet 在设置新的值之前调用
* didSet  在设置新的值之后调用

>

	class Hi {
		var total:Int = 0 {
			// 在设置新的值之前调用
	       willSet {
	           print(total, newValue)
	       }
	       // 在设置新的值之后调用
	       didSet {
	           print(total)
	       }
       }
	}

>

***类型属性***

*使用关键字static来定义值类型的类型属性，使用关键字class来为类定义类型属性*

* static 在枚举、结构体中修饰的属性、方法
* class 在类中修饰的属性、方法

*注意：也就是静态属性与方法，在类中使用(class)在枚举和结构体中使用(static)*

	struct Struct {
	    static var a = "Hello"
	    static var b:Int {
	        return 0
	    }
	}
	
	enum Enum {
	    static var a = "Hello"
	    static var b:Int {
	        return 0
	    }
	}
	
	class Class {
	    class var a:Int {
	        return 0
	    }
	    class func b() -> Int {
	        return 0
	    }
	}
	
*注意：类型属性是通过类型本身获取和设置的，而不是通过实例*

>

---

>

### 方法

>

***在实例中修改值类型***

*结构体和枚举是值类型，值类型的属性是不能在它的实例方法中被修改，但是 ...*

*可以使用变异方法从方法内部改变它的属性（值），并且它做的任何改变在方法结束时还保留在原始结构中*

	struct Hi {
	    var a:Int
	    var b:Int
	    func change() {
	        a = 10 // Error
	        b = 10 // Error
	    }
	}
	
	struct Hi {
	    var a:Int
	    var b:Int
	    // 使用变态方法
	    mutating func change() {
	        a = 10 // OK
	        b = 10 // OK
	    }
	}
	
	*注意：不能在结构体类型常量上调用变异方法，常量属性不能被改变*
	
>

***在变异方法中给self赋值***

*变异方法能够赋值给隐含属性self一个全新的实例*
	
	struct Hi {
	    var x = 0, y = 0
	    mutating func change() {
	    	 // 赋值一个全新的实例
	        self = Hi(x: 10, y: 10)
	    }
	}
	
>

---

>

### 下标

>

*定义下标使用subscript关键字*

*对于同一个类、结构体或枚举可以定义多个下标，通过索引值类型的不同来进行重载，而且索引值的个数可以是多个*

>

	struct Hi {
	    subscript(index:Int) -> Int {
	        get {
	            if index == 0 {
	                return 100
	            } else {
	                return 1000
	            }
	        }
	        set {
	            print(newValue)
	        }
	    }
	}
	
	// 实例
	var h = Hi()
	// 赋值调用 Set
	h[0] = 100
	// 读取调用 Get
	print(h[0])
	
*只读属性，可以直接将原本写在Get代码块中的代码写在subscript中*

	struct Hi {
	    subscript(index:Int) -> Int {
	        if index == 0 {
	            return 100
	        } else {
	            return 1000
	        }
	    }
	}

>

*下标是用来访问集合、列表或序列中元素的快捷方式*

	// 通过下标来访问类、结构体、枚举中的数组

	struct Matrix {
	    
	    var grid = [Int : String]()
	    subscript(index:Int) ->String {
	    get {
	        return grid[index]!
	    }
	    set {
	            grid.updateValue(newValue, forKey: index)
	    }
	    }
	    
	}
	
	var m = Matrix(grid: [Int : String]())
	m[0] = "Hello"
	print(m[0])

>

---

>

### 继承

>

*一个类可以继承另一个类的方法、属性、其它特征*

*继承是区分类与其它类型（结构体、枚举）的一个基本特征*

*类可以调用和访问超类的方法、属性、下标、并且可以重写这些方法、属性、和下标，以优化或修改它们的行为*

*可以为继承来的属性添加观察器，这样一来当属性值发生改变的时候，类就会被通知到*
b
*可以为任何属性添加观察器，无论它原本被定义为存储属性还是计算属性*

*子类只允许修改从超类继承来的变量属性，而不能修改继承来的常量属性*

>

---

>

### 重写

>

*子类可以为继承来的实例方法、类方法、实例属性、或下标提供自己定制的实现，我们把这种行为叫作重写*

*如果要重写某个特征，需要在重写定义的前面加上override关键字*

*访问超类（父类）的方法、属性、下标可以使用（super.xxx）*

***重写方法***

*在子类中可以重写继承来的实例方法或类型方法（静态方法）*

***重写属性***

*子类并不知道继承来的属性是存储属性还是计算属性，它只知道继承来的属性会有一个名字和类型*

*重写一个属性时，必须将它的名字和类型都写出来，这样才能使编译器去检查 ...*

*可以将一个继承来的只读属性重写为一个读写属性，只要重写时提供Set和Get即可*

*注意：不可以将一个只读属性重写为只读属性*

***重写属性观察器***

*不可以为继承来的常量存储属性或继承来的只读计算属性添加属性观察器，这些属性的值是不可以被设置的*

*不可以同时提供重写的set和重写属性观察器*

>

---

>

### 防止重写

>

*可以通过把方法、属性或下标标记为final来防止它们被重写*

*可以通过在关键字class前加final特征来将整个类标记为final，这样的类是不可被继承的*

>

---

>

### 构造

>

***init***

	class Hi {
		init() {
			...
		}
		init(a:Int,b:String) {
			...
		}
	}

*构造是为了使用某个类、结构体或枚举的实例而进行的准备过程, 这个过程包括为实例中的每个属性设置初始值和为其执行必要的准备额初始化任务*

*注意：类和结构体在实例创建时，必须为所有存储属性设置合适的初始值，存储属性的值不能处于一个为止状态*

*注意：当存储属性设置默认值或在构造中为其赋值时，它们的值会被直接设置，不会出发任何属性检查器*

*如果你在定义构造器时没有提供参数外部名字，系统会为每个构造器的参数自动生成一个跟内部名字相同的外部名*

*构造器参数不使用外部名称，可以使用下划线代替外部参数名称即可*

***可选属性类型***

*可选类型属性将自动初始化为空nil, 表示这个属性是故意在初始化时设置为空的*

	class Hi {
		// 可选类型
		var a:String?
		// 构造也没有为其赋值
		init() { }
	}

---