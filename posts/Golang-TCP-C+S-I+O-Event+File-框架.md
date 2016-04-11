---
title: Golang TCP C+S I+O Event+File 框架
date: '2014-08-07'
description:
categories:

tags:golang

---

介绍:

框架针对C/S模式,一个Server与多个Client


一,支持Server向多个Client同时(并发)发送事件通知


二,支持Server向多个Client同时(并发)发送数据文件


三,支持Server接收(并发)多个Client发送的事件通知


四,支持Server接收(并发)多个Client发送的数据文件

---

Server 结构(如何发送消息事件与文件):

	// 注意:(socket.TCP)来自import
	// socket "github.com/nulijiabei/msocket"

	// 根据 TCP 实现接口
	type TCP struct {
	 	Addr     string       // 要连接地址
		Port     string       // 要连接端口
		Conn     *net.TCPConn // 当前的连接，如果 nil 表示没有连接
		maxRetry int          // 最大重试次数
	 }

	// 初始化时需要登记Client的IP和端口与连接
	// 请约定好 Server -> Client 所是用的端口
 	// 请约定好 Client -> Server 所使用的端口

	// 在 Server 中全局保存 Client 连接
	SlaConn map[int]*socket.TCP // Slaver-TCP [节点]

>

	// 多线程内各线程状态
	type NodeStatus struct {
		Ok   bool        `json:"ok"`
		ID   int         `json:"id"`
		IP   string      `json:"ip"`
		Data interface{} `json:"data"`
	}

	// 处理所有连接并返回报告
	func ControlConns(callback func(id int, tcp *socket.TCP) error) map[string]NodeStatus {
		// 返回结构
		maxData := make(map[string]NodeStatus)
		// 通过程道控制线程在所有client发送结束后继续
		maxSla := make(chan NodeStatus)
		// 遍历所有连接，发送结构
		for k, v := range SlaConn {
			// 线程中对所有机器发送
			go (func(id int, tcp *socket.TCP, ch chan NodeStatus) {
				// 返回结构
				ns := new(NodeStatus)
				ns.Ok = true
				ns.ID = id
				ns.IP = tcp.Addr
				// 闭包
				if err := callback(id, tcp); err != nil {
					ns.Ok = false
					ns.Data = err.Error()
				}
				// 保证在退出时告知称道
				defer (func() {
					ch <- *ns
				})()
			})(k, v, maxSla)
		}
		// 线程计数器
		var i int = len(SlaConn)
		for i != 0 {
			select {
			case rd := <-maxSla:
				maxData[rd.IP] = rd
				i -= 1
			}
		}
		// 完成返回
		return maxData
	}

>

	// 通过 ControlConns 闭包多线程,高效的处理连接

	// 通过闭包,并发处理连接
	ControlConns(func(id int, tcp *socket.TCP) error {
		// 发送结构
		if err := tcp.ReadWrite(func(conn *net.TCPConn) error {
			// 协议发送
			return ProtocolSend(conn, "event", map[string]string{"data": "hello", "")
		}); err != nil {
			// 判断是否异常
			return err
		}
	}

	// 使用 ProtocolSend 发送事件消息与文件方法
	// func ProtocolSend(conn net.Conn, types string, head map[string]string, file string) error
	// 通过以上声明可以知道,head参数为MAP,可以将消息事件封装为一个MAP来进行
	// 将KEY作为事件名称,例如:"名字:NLJB"
	// 其中"Event"为必填项,在Client中可以通过这个必填项类型,来决定消息发给哪个函数
	// 当然,你也可以同时发送一个文件过去,将文件路径("/root/file")作为参数即可
	ProtocolSend(conn, "event", map[string]string{"name": "nljb", "/root/file")

	// 需要注意是,快速事件不建议携带文件,文件可以单独发送
	// 因为,处理文件会减慢,事件的处理时间

---

Server 结构(如何接收消息事件与文件):

	// 这里将(8080)作为Client->Server的端口号,所以Server要监听这个端口

	// 接收 Client 发来的所有TCP请求
	func doReadTcpSla(ch chan int) {

		// 监听TCP端口等待注册
		lis, err := socket.NewListen("", "8080", 3).ListenTCP()
		if err != nil {
			log.Fatalln(err)
		}
		// 保证监听正常关闭
		defer lis.Close()

		// 等待连接
		for {
			// 等待 client 连接
			conn, err := lis.Accept()
			if err != nil {
				log.Println(err)
				continue
			}
			// 接收连接线程
			go (func(conn net.Conn) {
				// 连接交给解释器
				res, err := ProtocolReceive(conn)
				if err != nil {
					log.Println(err)
				} else {
					// 结果交给被动事件处理
					err := Passive(conn.RemoteAddr().String(), res)
					if err != nil {
						log.Println(err)
					}
				}
			})(conn)
		}
	}

>

	// 连接被接收后会送到,中间件ProtocolReceive处理,处理后送给Passive
	// 在这里你就可以为所欲为了

	// 我是这样做的,把所以类型定义为方法,保存到handlers里
	// 通过事件中的"type"来调用,将其余参数发给这个函数(event)

	// 被动事件处理
	func Passive(addr string, res map[string]string) error {
		// 获取事件类型(所有事件均需要存在)
		label, _ := res["type"]
		// 从MAP中取出执行方法
		handler, hasHandler := handlers[label]
		// 判断方法是否存在
		if !hasHandler {
			return fmt.Errorf(fmt.Sprintf("unknowns func [%s]"), label)
		}
		// 执行方法
		err := handler(addr, res)
		if err != nil {
			return err
		}
		// 成功,返回
		return nil
	}

	// 看到这里如果你还不知道如何拿到你上面发出来的{"data":"hello"} ...
	// res map[string]string 里面存储的就是你发出来的所有(KEY:VALUE)了
	// addr 里面存储的是谁发过来的消息(IP)

	// 至于文件呢,中间件会添加两个KEY,你可以通过内容得到路径
        -> head["file"] = file
        -> head["md5"] = z.MD5(file)


---

Client 结构(如何发送消息事件与文件):

	// 你可能已经了解了Server是如何发送消息的,其实,Client也是相同的方法
	// 唯独不同的是,没有多个Server,只有一个
	MasConn *socket.TCP // master-TCP 网络连接

---

Client 结构(如何接收消息事件与文件):

	// 同样,你也看过了,Server是如何接收Client的消息的

	// 这里将(9090)作为Server->Client的端口号,所以Client要监听这个端口

	// 同样的ProtocolReceive解析,和Passive处理,只是少了多线程

	// 接收 Server 所有TCP请求
	func doReadTcpMas(ch chan int) {

		// 监听TCP端口等待注册
		lis, err := socket.NewListen("", "9090", 3).ListenTCP()
		if err != nil {
			log.Fatalln(err)
		}
		// 保证监听正常关闭
		defer lis.Close()

		// 等待连接
		for {

			// 等待 client 连接
			conn, err := lis.Accept()
			if err != nil {
				log.Println(err)
				continue
			}

			// 解释数据
			res, err := ProtocolReceive(conn)
			if err != nil {
				// 打印错误
				log.Println(err)
			} else {
				// 被动事件处理
				err := Passive(conn.RemoteAddr().String(), res)
				if err != nil {
					// 打印错误
					log.Println(err)
				}
			}

		}
	}

>

	// 同样的同样 ...
	// 被动事件处理
	func Passive(addr string, res map[string]string) error {
		// 获取事件类型
		label, _ := res["type"]
		// 从MAP中取出执行方法
		handler, hasHandler := handlers[label]
		// 判断方法是否存在
		if !hasHandler {
			return fmt.Errorf(fmt.Sprintf("unknowns func [%s]", label))
		}
		// 执行方法
		err := handler(addr, res)
		if err != nil {
			return err
		}
		// 成功,返回
		return nil
	}	

---

中间件结构(发送):

	// 注意:引入 z 库方法
	// z "github.com/nutzam/zgo"

	// 本协议适用 Server 与 Client ,负责发送结构数据
	func ProtocolSend(conn net.Conn, types string, head map[string]string, file string) error {
		// 发送数据
		var data string
		// Header
		if head == nil {
			// Make
			src := make(map[string]string)
			// 赋值
			head = src
		}
		// 提取本地IP地址
		x := strings.LastIndex(fmt.Sprintf("%s", conn.LocalAddr()), ":")
		// 保存本地IP地址
		head["ip"] = fmt.Sprintf("%s", conn.LocalAddr())[:x]
		// Header Type
		head["type"] = types
		// 需发送的文件是否存在
		if !z.IsBlank(file) && !z.Exists(file) {
			return fmt.Errorf("not found file")
		}
		// 如需发送文件添加标签,设置头信息
		if !z.IsBlank(file) {
			head["file"] = file
			head["md5"] = z.MD5(file)
		}
		// 遍历,组合头信息
		for k, v := range head {
			data += (k + "#WWW.NLJB.NET#" + v + "\n")
		}
		// 如需发送文件添加标签,组合头信息
		if !z.IsBlank(file) {
			data += "\n\n"
		}
		// 发送文件头
		_, err := conn.Write([]byte(data))
		if err != nil {
			return err
		}
		// 判断是否有文件需要发送
		// [0] 远端文件不存在需要发送
		// [1] 远端文件存在不需要发送
		if !z.IsBlank(file) {
			// 等待是否需要发送消息
			line, _ := bufio.NewReader(conn).ReadString('\n')
			// 判断是否需要发送
			if z.ToInt(z.Trim(line), 1) == 0 {
				// 打开文件
				f := z.FileR(file)
				// 判断是否打开成功
				if f != nil {
					// 建立读取流
					rd := bufio.NewReader(f)
					// 建立写入流
					wd := bufio.NewWriter(conn)
					// 写入
					_, err := io.Copy(wd, rd)
					if err != nil {
						return err
					}
					// 把缓冲区的数据强行输出
					wd.Flush()
					// 保证文件正常关闭
					f.Close()
				}
			}
		}
		// 发送完成
		return nil
	}

---

中间件结构(接收):

	// 本协议适用 Server 与 Client ,解析TCP传输结构
	func ProtocolReceive(conn net.Conn) (map[string]string, error) {
		// 存放解释后的结构
		var res map[string]string = make(map[string]string)
		// 建立读取流
		rd := bufio.NewReader(conn)
		// 空行计数器
		var i int = 0
		// 在循环中进行读取
		for {
			// 按行读取
			data, err := rd.ReadString('\n')
			if err == io.EOF { // 遇到结尾跳出
				break
			} else if err != nil { //遇到错误
				return nil, err
			}
			// 去除字符串结尾的换行
			var str string = z.Trim(data)
			// 如果出现空行,计数器加一
			if z.IsBlank(str) {
				i += 1
			} else { // 否则计数器,归零
				i = 0
			}
			// 如果没有出现空行
			if i == 0 {
				// 拆分字符串
				value := strings.Split(str, "#WWW.NLJB.NET#")
				// 将字符串存入结构
				res[value[0]] = value[1]
			}
			// 如果出现两个空行,则开始写入文件
			if i == 2 {
				// 获取文件名称
				fname, _ := res["file"]
				// 获取文件MD5
				fmd5, _ := res["md5"]
				// 判断文件是否存在
				// [0] 远端文件不存在需要发送
				// [1] 远端文件存在不需要发送
				if !z.Exists(fname) || z.MD5(fname) != fmd5 {
					// 需要这个文件
					conn.Write([]byte("0\n"))
					// 收取文件
					f := z.FileW(fname)
					// 建立写入流
					wd := bufio.NewWriter(f)
					// 从连接读取并写入
					_, err := io.Copy(wd, rd)
					if err != nil {
						return nil, err
					}
					// 把缓冲区的数据强行输出
					wd.Flush()
					// 关闭文件
					f.Close()
				} else {
					// 不需要这个文件
					conn.Write([]byte("1\n"))
					// 跳出
					break
				}
				// 验证
				if z.Exists(fname) && z.MD5(fname) == fmd5 {
					break
				} else {
					return nil, fmt.Errorf("file finger fail")
				}
			}
		}
		// 返回
		return res, nil
	}
