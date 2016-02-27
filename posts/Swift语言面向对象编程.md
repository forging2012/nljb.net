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

* 等价于（ === ）
* 不等价于（ !== ）

>

---