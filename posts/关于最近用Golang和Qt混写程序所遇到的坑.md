---
title: 关于最近用Golang和Qt混写程序所遇到的坑
date: '2015-01-31'
description:
categories:

tags:golang

---

### 先说 Mac os 下面吧

>

一路上顺风顺水。

>

总结了一下，CGO所使用的原理，在Go里面调用C的程序

其实就是使用了C++的一个特性，extern 

	// 网上是这样解释的
	extern可置于变量或者函数前，以表示变量或者函数的定义在别的文件中
	提示编译器遇到此变量和函数时在其他模块中寻找其定义。

当你对一个函数 extern 声明后，这个函数你就可以使用了，但是其实它并不存在，需要Go去生成

// Go 生成有个前地，就是在这个函数上面加一行(//export cgo_connect)，cgo_connect 就是函数名

但是当使用的时候，其实Go已经生成好了，编译到程序里面，所以运行的就是所以 Cgo 的程序

	// 可以通过一个命令来看 Go 中间做了什么
	go tool cgo xxx.go
	// 这 xxx.go 里面要写的就是下面这些C与Go的混合代码
	/*
	// 这里看到 extern 了把，使用这个的函数，只有三个地方存在
	// 一，注释内
	// 二，Go 内用 //export drv_cgo_connect 实现的
	// 三，在 C 里面实现的（可能是文件，也可能是库) 
	extern void drv_cgo_connect(int (*)(void *, int));
	static void init_callback()
	{
		extern int cgo_connect(void *, int);
		drv_cgo_connect(&cgo_connect);
	}
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -lexamples // 库内有一切，不仅调用，还可以互访
	import "C"
	import "unsafe"

	func start() {
		C.init_callback()
	}
	
	// 这里就是生成了一个 C 的 cgo_connect 函数
	// 想要在 C 中使用这个函数，就需要在 C 里面 extern int cgo_connect(...) 才可以.
	//export cgo_connect 
	func cgo_connect(_content unsafe.Pointer, _size C.int) C.int {
		device := string(C.GoBytes(_content, _size))
		log.Println("Go->", device)
		if err := StaticConn.Connect(device); err != nil {
			return C.int(0)
		}
		return C.int(1)
	}

这样一来，你就可以在 Go 里面调用 C 里面实现的函数

同样原理，如果 C 想要调用 Go 的函数，相同方法，只是把 extern 调换一下!

---

上面简单的了解了一下，CGO 原理，现在说一下具体情况，比如 QT 这样庞大的程序

>

对于 QT 这样的大程序，而且涉及到 QMake 编译，所以使用 CGO 本身混编(.go,.c.,.h)就没戏了

所以怎么办呢，这位同学说对了，就是这样，使用库（windows是dll)(linux是so)(mac是dylib)

将 QT 的 .Pro 文件改一下 TEMPLATE = lib 这样就会生成库文件

但是上面的同学问了，库文件能被Go调用，那库文件怎么调用Go呢

>

问题就来了，我试验了无数次。坑，坑，坑！

你想到了，extern 没错，但是 extern 的函数需要在本库中找到实现的函数

对于Cgo里面，你没有实现是因为你设置了 //export 所以 Go 就帮你做了

所以怎么办，对了，你想到，go tool cgo 把 C 编译出来，导入到 QT 中不就了。

错，大错特错，编译出来的文件，不完整，可远观，不可近玩嫣！

我试的时候虽然能编译过，但是 Go 转 C 的函数根本没运行 

>

这样一来，楼上的同学哭了，那怎么办啊。

>

当当当当～！在这里用到了 C++ 的另外一个关键字 inline 隆重出马

	// 网上这样介绍的
	inline关键字用来定义一个类的内联函数，引入它的主要原因是用它替代C中表达式形式的宏定义。

也就是说，你在C库中用inline定义一个空函数，然后使用TA，然后在Go里面用//export 定义一个同名函数。

这样一来，C里面就使用的不是inline定义的函数，而是你Go里面的函数了，天哪，太简单，太方便了。

>

<img src="{{urls.media}}/关于最近用Golang和Qt混写程序所遇到的坑/1.png" alt="" width="600">

---

### 再说 Windows 这个坑吧

>

当你 Mac 或者 Linux 按照上面方法，写完了，你可能兴高采烈的跑去 windows 编译

当你 费了 九牛二虎之力，把环境编译好了，之后呢，编译呗，坑坑坑，恭喜，你又掉坑里了。

这个问题，我找啊找，查啊查，费了就牛二老之力，找到了问题了。

	问题就是 inline  // 网上是这样说的
	inline说明对编译器来说只是一种建议，编译器可以选择忽略这个建议。
	比如，你将一个长达1000多行的函数指定为inline，编译器就会忽略这个inline，将这个函数还原成普通函数。

天哪，看编译器心情吗，在 Mac 里面就用 Go 里面 原始函数，在 Windows 下面就选择了，inline 定义的空函数，这可怎么办啊。

网上一顿乱找，什么，强制 inline ，编译器不优化，一顿乱找，搞不定啊。

后来听说，windows 对 dll 有特殊限制，有些 关键字无法传递，比如 inline ,所以，唉！！！，没办法.

既然不用 inline 也就不能用 extern 因为 只有加了 extern inline 的函数才变成实际存在的，如果去掉 inline

编译器会一直提示你，没有找到，没有找到，没有找到。啊啊啊啊啊啊啊！

---

找啊找，查啊查，坑啊坑。看了一些别人的代码，涉及到的太少，几乎没有

翻了一下 liteide 的代码，哈哈，看到曙光了，怎么办呢

是怎么处理的呢，比较复杂，但是可行

就是在 Cgo 里面自己调用自己的函数，然后 CGo 的方法，把函数指针传到库里去，在库里面搞个全局保存一下.

	/*
		extern void drv_cgo_connect(int (*)(void *, int));
		static void init_callback()
		{
			extern int cgo_connect(void *, int);
			drv_cgo_connect(&cgo_connect);
		}
	*/
	// #include <stdio.h>
	// #include <stdlib.h>
	// #cgo LDFLAGS: -L./ -lexamples
	import "C"
	import "unsafe"

	func start() {
		C.init_callback()
	}

	//export cgo_connect
	func cgo_connect(_content unsafe.Pointer, _size C.int) C.int {
		device := string(C.GoBytes(_content, _size))
		log.Println("Go->", device)
		if err := StaticConn.Connect(device); err != nil {
			return C.int(0)
		}
		return C.int(1)
	}

	// 在 C 里面是这样

	typedef int (*COMMAND_CGO_CONNECT_FUNCTION)(void *, int);

	typedef int (*COMMAND_CGO_CHECKCONN_FUNCTION)();

	COMMAND_CGO_CONNECT_FUNCTION cgo_connect;

	COMMAND_CGO_CHECKCONN_FUNCTION cgo_checkconn;

	extern "C" void drv_cgo_connect(int (*_a)(void *, int))
	{
	    cgo_connect = _a;
	}

	extern "C" void drv_cgo_checkconn(int (*_a)())
	{
	    cgo_checkconn = _a;
	}


这样一来，问题解决了，但是复杂了一些，对 Cgo 减10分.

---

补充，当 windows 运行的时候 会显示 DOS 窗口，怎么办呢 

	go build -ldflags -H=windowsgui 
	go build -ldflags -H=windowsgui XXX.go

---

再次补充：


	// 可以把函数当指针传，这样就不需要那么多的 drv_cgo_xxx
	/*
		extern void cgo_init();
		extern int cgo_start();
		extern void cgo_callback(void *);
		extern void drv_cgo_callback(int, void*);
		extern void drv_cgo_callback_2(int, void*);
		static void init_callback()
		{
			int _cgo_connect = 1;
			int _cgo_checkconn = 2;
			int _cgo_disconn = 3;
			int _cgo_command = 4;
			int _cgo_shortcuts = 5;
			int _cgo_message = 6;
			extern int cgo_connect(void *, int);
			extern int cgo_checkconn();
			extern void cgo_disconn();
			extern void cgo_command(void *, int);
			extern void cgo_shortcuts(void *, int);
			extern void * cgo_message();
			drv_cgo_callback_2(_cgo_connect, &cgo_connect);
			drv_cgo_callback_2(_cgo_checkconn, &cgo_checkconn);
			drv_cgo_callback(_cgo_checkconn, &cgo_checkconn);
			drv_cgo_callback(_cgo_disconn, &cgo_disconn);
			drv_cgo_callback(_cgo_command, &cgo_command);
			drv_cgo_callback(_cgo_shortcuts, &cgo_shortcuts);
			drv_cgo_callback(_cgo_message, &cgo_message);
		}
	*/

	typedef int (*COMMAND_CGO_CONNECT_FUNCTION)(void *, int);

	typedef int (*COMMAND_CGO_CHECKCONN_FUNCTION)();

	static COMMAND_CGO_CONNECT_FUNCTION cgo_connect = 0;

	static COMMAND_CGO_CHECKCONN_FUNCTION cgo_checkconn = 0;

	extern "C" void drv_cgo_callback_2(int _a, void * _b)
	{
	    /*
	    int _cgo_connect = 1;
	    int _cgo_checkconn = 2;
	    */
	    switch (_a) {
	    case 1:
		cgo_connect = (COMMAND_CGO_CONNECT_FUNCTION)_b;
		break;
	    case 2:
		cgo_checkconn = (COMMAND_CGO_CHECKCONN_FUNCTION)_b;
		break;
	    }
	}

>

<img src="{{urls.media}}/关于最近用Golang和Qt混写程序所遇到的坑/2.png" alt="" width="600">
