---
title: Swift语言面向对象编程
date: '2016-02-26'
description:
categories:

tags:ios

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

>

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

*只读计算属性的声明可以去掉Get关键字和花括号
	
	var a:Hello {
		return Hello(x: 0, y: 0)
	}

>

***属性监视器***

>

*属性监视器监控和响应属性值的变化*

*每次属性被设置的时候都会调用属性监视器，甚至新的值和现在的值相同的时候也不例外*

* willSet 在设置新的值之前调用
* didSet  在设置新的值之后调用

>

	class Hi {
		var total:Int = 0 {
	       willSet {
	           print(total, newValue)
	       }
	       didSet {
	           print(total)
	       }
       }
	}

>

---