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

*可以为任何属性添加观察器，无论它原本被定义为存储属性还是计算属性*

*子类只允许修改从超类继承来的变量属性，而不能修改继承来的常量属性*

	class Hi:Pro {
	    // 接口方法
	    func sayHello() -> String {
	        return "Hello"
	    }
	}

>

---

>

### 重写

>

*子类可以为继承来的实例方法、类方法、实例属性、或下标提供自己定制的实现，我们把这种行为叫作重写*

*如果要重写某个特征，需要在重写定义的前面加上override关键字*

*访问超类（父类）的方法、属性、下标可以使用（super.xxx）*

	class Hello:Hi {
	    // 重写方法
	    override func sayHi() {
	        // 执行父类方法
	        super.sayHi("Hello")
	        // ...
	       print("Hello \(name)")
	    }
	}

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

***构造中修改常量属性***

*只要在构造结束前常量的值能确定，就可以在构造总的任意时间点修改常量的属性值*

	class Hi {
	    let a:Int
	    init() {
	        self.a = 99
	    }
	}

***值类型的构造器代理***

*构造器可以通过调用其它构造器来完成实例的部分构造，这一过程成为构造器代理*

	struct Hi {
	    init() {
	        print("...")
	    }
	    init(index:Int) {
	        self.init()
	        print(index)
	    }
	}
	
***类的继承和构造***

*每一个类都必须至少拥有一个指定构造器*

*类中所有存储属性，包括所有继承自超类的属性，都必须在构造中设置初始值*

*系统提供两种类型的构造来确保所有类实例中的存储属性都能获得初始值(指定构造器、便利构造器)*

*便利构造器是类中次要的、辅助型的构造器，可以定义便利构造器来调用同一个类中的指定构造器，并为其参数提供默认值，也可以定义便利构造器来创建一个特殊用途或特定输入参数的实例*

>

* 子类构造器必须调用直接父类的指定构造器
* 便利构造器必须调用同一个类中的其它构造器 
* 便利构造器调用的构造器链的最终节点必须是指定构造器

>

	class Hello {
	    init(index:Int) {
	        print("init hello = \(index)")
	    }
	}
	
	
	class Hi:Hello {
	    
	    var a:Int, b:Int
	    
	    init(a:Int,b:Int) {
	        self.a = a
	        self.b = b
	        // 子类必须调用直接父类的指定构造器(除非init(){})
	        // 必须最后一步调用
	        super.init(index: a)
	    }
	    
	    convenience init(a:Int,b:Int,c:Int) {
	    	  // ...
	    	  // 便利构造器必须调用一个self.init(...)
	    	  // 必须最后一步调用 ...
	        self.init(a: a, b: b + c)
	    }
	    
	}
	
	// 使用指定构造器初始化
	var h = Hi(a: 1, b: 1))
	// 使用便利构造器初始化
	var h = Hi(a: 1, b: 1, c: 1)

>

***构造器的继承和重载***

*系统中的子类不会默认继承超类的构造器*

***自动构造器的继承***

*如果子类没有定义任何指定构造器，它将自动继承超类的指定构造器*

	class Good {
		 // 构造器
	    init(str:String) {
	        print(str)
	    }
	}
	
	// 子类没有定指定构造器
	class Hi:Good {
	    // 自动继承所有超类的指定构造器
	}
	
	// 子类没有定指定构造器
	class Hello:Hi {
	    // 自动继承所有超类的指定构造器
	}
	
	var h = Hello(str: "Hello")
	
*如何子类提供了所有超类指定的构造器的实现，它将自动继承超类的便利构造器, 不管是通过啥们上面方法自动继承还是通过自定义实现的，它将自动继承所有超类的便利构造器*

	class Hi {
	    init() {
	        
	    }
	    init(index:Int) {
	        print(index)
	    }
	    convenience init(str:String) {
	        self.init()
	        print(str)
	    }
	}
	
	class Hello:Hi {
	    override init() {
	        super.init()
	    }
	    override init(index: Int) {
	        super.init(index: index)
	    }
	}
	
	// 继承了超类的便利构造器
	var h = Hello(str: "hello")
	
***要求实现或重写构造器***

*表示每一个子类都必须实现这个构造器*

	class Hi {
		required init() {
			...
		}
	}
	
***通过闭包或函数来设置属性的默认值***

*在闭包执行时，实例的其它部分都还没有初始化，这意味着不能再闭包里访问其它属性，就算这个属性有默认值也不可以，也不能使用self属性，或其它实例方法*

	class Hi {
		// 闭包来设置属性值
		let a:Int {
			return 0
		}
	}
	
>

---

>

### 析构

>

*在一个类的实例被释放之前，析构函数被立即调用, 用关键字deinit来标识析构函数*

*每个类最多只能有一个析构函数，析构函数不带任何参数*
	
	deinit {
		....
	}

>

---

>

### 类型转换

>

*类型转换是一种检查实例的方式，让实例作为它的超类或者子类的一种方式*

*类型转换使用is和as操作实现，提供一种简单的方式去检查值的类型或者转换它的类型, 也可以用来检查一个类是否实现了某个协议*

***类型检查***

*is*

*用类型检查操作符(is)来检查一个实例是否属于特定的子类型*

***向下转换***

*as*

*某类型的一个常量或变量可能属于一个子类，可以尝试向下转换到它的子类型，可用类型转换操作符(as)*

	class Hi {
	    func a() {
	        print("a ...")
	    }
	}
	
	class Hello:Hi {
	    func b() {
	        print("b ...")
	    }
	}
	
	class World:Hi {
	    func c() {
	        print("c ...")
	    }
	}
	
	let libs = [Hello(), World(), Hello(), World()]
	for item in libs {
	    // item Type is Hi
	    // 所以需要向下转换类型
	    item.a()
	    if let x = item as? Hello {
	        x.b()
	    } else if let y = item as? World {
	        y.c()
	    }
	}
	
*注意：可选绑定(if let x = item as? Hi)*

***Any和AnyObject的转换***

* AnyObject 可以代表任何class类型实例
* Any 可以表示任何类型，除了方法类型(func type)

>

* AnyObject 用于任何类实例
* Any 用于任何变量

***AnyObject***

*当需要工作中使用Cocoa API，它一般接收一个[AnyObject]类型的数组，或者说"一个任何对象类型的数组"*

	let obj:[AnyObject] = [Hello(), World()]
	
***Any***

*使用Any类型来混合不同类型一起工作, 但是方法类型不可以*

	var things = [Any]()
	things.append(0)
	things.append(0.0)
	things.append(42)
	things.append(3.1415926)
	things.append("hi")
	things.append(3.0, 5.0)
	things.append(Hello())
	things.append(World())

>

---

>

### 扩展

>

***扩展可以:***

* 添加计算属性和静态计算属性
* 定义实例方法和类型方法
* 提供新的构造器
* 定义下标
* 定义和使用新的嵌套类型
* 使一个已有类型符合某个协议

>

*一个扩展可以扩展一个已有类型，使其能够适配一个或多个协议*
	
	extension SomeType: SomeProtocol, AnotherProctocol {
		...
	}
	
***扩展构造器***

*扩展可以向已有类型添加新的构造器，这可以让你扩展其它类型，将自己定制类型作为构造器参数, 或者提供该类型的原始实现中没有包含的额外初始化选项*

	struct Rect {
		...
	}
	
	// 添加新的构造器
	extension Rect {
	    
	    init(a:Int, b:Int) {
	      ...  
	    }
	    
	}

***扩展方法***

	class Hi {
	    
	}
	
	extension Hi {
	    func say() {
	        print("say Hi")
	    }
	    class func hello() {
	        print("say Hello")
	    }
	}
	
	var h = Hi()
	h.say()
	Hi.hello()
	
***扩展下标***

	extension Hi {
		subscript(index:Int) -> Int {
			return 0
		}
	}
	
***扩展嵌套类型***

	extension Character {
		// 在Character中嵌套一个Kind的枚举
		enum Kind {
			...
		}
	}

>

---

>

### 协议（接口)

>

*协议用于统一方法和属性的名称，而不实现功能，协议能够被类、枚举、结构体实现，满足协议条件的被称为遵循者*

*遵循者需要提供协议指定成员，如属性、方法、操作符、下标等*

	protocol SomeProtocol {
	    // ...
	}

	class SomeClass: SomeProtocol {
	}
	
*当某个类含有超类的同时实现了协议，应吧超类放在所有的协议之前*

	class SomeClass: SomeSuperClass, SomeProtocol {
	}
	
***协议属性要求***

*在属性声明后写上{get set}表示属性为可读写的*

	// 只限协议里面才可以这样设置
	protocol SomeProtocol {
	    var a:Int {get set}
	}

* 用类来实现协议时，使用class来表示该属性为类成员（静态）
* 用结构体和枚举来实现协议时，使用static来表示 ...

>
	
	protocol SomeProtocol {
	       class var a:Int { get set }
	}
		
	protocol SomeProtocol {
	    static var b:Int { get set }
	}
	
>

***方法要求***

*协议方法支持可变参数，不支持默认参数*

	protocol SomeProtocol {
	    // 不支持, 默认参数
	    func say(a:String = "hi"){
	        // Error
	    }
	}
	
	// 可变参数，可以使一个参数接受零个或多个指定类型的值
	func sayNumber(numbers: Double...){
	    for number in numbers {
	        print(number)
	    }
	    
	}
	sayNumber(1, 2, 3, 4, 5)

>

***可变方法要求***

*能在方法或函数内部改变实例类型的方法称谓可变方法*

*在值类型（结构体和枚举）中的函数前加上mutating关键字来表示该函数允许改变该实例和其属性的类型*

*类中的成员为引用类型，可以方便地修改实例及其属性的值而无需改变类型*

*结构体和枚举中的成员为值类型，修改变量的值就相当于修改变量的类型，而系统不允许修改类型，因此需要前置mutating来表示该函数中能够修改类型*

* 用class实现协议中的mutating方法时，不用写mutating关键字
* 用结构体、枚举实现协议中的mutating方法时，必须写mutating关键字

>

	// Error
	struct Hi {
	    var a:Int = 0
	    var b:Int = 0
	    // 这样是错误的，不允许修改
	    func change(a:Int, b:Int) {
	        self.a = a
	        self.b = b
	    }
	}

	// OK
	struct Hi {
	    var a:Int = 0
	    var b:Int = 0
	    // 这样是OK的
	    mutating func change(a:Int, b:Int) {
	        self.a = a
	        self.b = b
	    }
	}
	
>

### 协议作为类型

>

*协议本身不实现任何功能，但可以将它当作类型来使用*

*使用场景:*

* 作为函数、方法、或构造器中的参数类型、返回值类型
* 作为常量、变量、属性的类型
* 作为序列、字典或其它集合项的类型

*协议类型应与其它类型（Int, String）的写法相同，使用驼峰式*

	protocol Protocol {
	    func sayMe()
	}
	
	class Hello:Protocol {
	    func sayMe() {
	        print("show me")
	    }
	}
	
	class Hi {
	    let p:Protocol
	    init(p:Protocol) {
	        self.p = p
	    }
	}
	
	var h = Hello()
	var x = Hi(p: h)
	x.p.sayMe()

>

***代理***

*代理是一种设计模式，它允许类或结构体将一些需要它们负责的功能交由给其它的类型*

*代理模式实现：定义协议来封装那些需要被代理的函数和方法，使其遵循者拥有这些被代理的函数和方法*

	protocol Hello {
	    var name:String? {get set}
	    func play()
	}
	
	// 2, 定义一个代理，让别人实现这个方法 ...
	protocol HelloDelegate {
	    func game(hello:Hello)
	}
	
	
	// 3, 继承了代理，就必须实现代理的方法
	class HelloTracker:HelloDelegate {
	    // 4, 实现代理方法
	    func game(hello:Hello) {
	        print(hello.name)
	    }
	}
	
	// 使用代理
	class Hi:Hello {
	    
	    var name:String?
	    
	    init(name:String) {
	        self.name = name
	    }
	    
	    // 代理变量
	    var delegate:HelloDelegate?
	    
	    func play() {
	        // 1, 需要一个Game方法，又不想实现
	        delegate?.game(self)
	    }
	    
	}
	
	var h = Hi(name: "nljb")
	h.delegate = HelloTracker()
	h.play()

>

***通过扩展添加协议一致性***

*即使无法修改

*通过扩展为已存在的类型添加协议时，该类型的所有实例也会随之添加协议中的方法*

	protocol Protocol {
	    func play()
	}
	
	class Dice {
	    
	}
	
	extension Dice:Protocol {
	    func play() {
	        print("扩展添加协议")
	    }
	}

***通过扩展补充协议声明***



---