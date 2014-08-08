---
title: Golang-做自己的动态网站
date: '2014-08-08'
description:
categories:

tags:golang

---

	// 制作自己的动态网站

	// 动态文件的路由和静态文件的路由是分开的
	// 动态文件使用http.HandleFunc进行设置
	// 静态文件使用http.Handle进行设置,需要使用http.FileServer

	// 在主方法中通过http.Handle增加静态路径
	// 在主方法中通过http.HandleFunc增加动态路径

	func main() {

		// 设置CPU核心数量
		runtime.GOMAXPROCS(runtime.NumCPU())

		// 设置日志的结构
		log.SetFlags(log.Lshortfile | log.Ldate | log.Ltime | log.Lmicroseconds)

		http.Handle("/css/", http.FileServer(http.Dir("template")))
		http.Handle("/js/", http.FileServer(http.Dir("template")))
		http.Handle("/files/", http.FileServer(http.Dir("template")))
		http.Handle("/images/", http.FileServer(http.Dir("template")))
		http.HandleFunc("/", index)

		// 建立监听
		if err := http.ListenAndServe(":8080", nil); err != nil {
			// 踢出错误
			log.Panic(err)
		}

	}

---

	// 其中 http.HandleFunc("/", index) 中 index 为:

	// 通过 r.ParseForm() 解码GET参数,如果出现admin则保存到cookie

	func index(w http.ResponseWriter, r *http.Request) {

		// 解析参数
		r.ParseForm()

		// 增加 cookie
		if _, ok := r.Form["admin"]; ok {
			// cookie
			cookie := http.Cookie{Name: "username", 
						Value: "admin",
						Expires: time.Now().Add(24 * time.Hour)}
			// cookie
			http.SetCookie(w, &cookie)
		}

		// 读取 cookie
		if cookie, err := r.Cookie("username"); err == nil {
			// 权限
			if cookie.Value == "admin" {
				// 管理员
				admin = "admin"
			}
		}


		// 解析主页面
		t, err := template.ParseFiles("template/index.html")
		if err != nil {
			// 输出错误信息
			http.Error(w, err.Error(), 500)
			return
		}

		// 执行
		t.Execute(w, nil)

		// 返回
		return

	}

---

	// 看到以上例子,你可能会想如何将内容放到动态网页的"template/index.html"中呢
	// 可以通过 t.Execute(w, nil) 中的,第二个参数

	// 下面是个列出目录文件到页面的例子

	type I struct {
		Id   int
		Name string
		Size string
		Date string
		Stat string
	}

	type D struct {
		// 文件列表
		Files []*I
	}

	// 构造
	func NewD() *D {
		d := new(D)
		d.Files = make([]*I, 0)
		return d
	}

	// 文件列表
	func filelist(w http.ResponseWriter, r *http.Request) {

		// 管理员
		var admin string

		// cookie
		if cookie, err := r.Cookie("username"); err == nil {
			// 权限
			if cookie.Value == "admin" {
				// 管理员
				admin = "admin"
			}
		}

		// 解析参数
		r.ParseForm()

		// 获取文件名称
		fname := z.Trim(r.FormValue("f"))

		// 创建返回对象
		d := NewD()

		// ID
		var id int

		// 遍历本地文件
		filepath.Walk("files", func(ph string, f os.FileInfo, err error) error {
			// 文件不存在
			if f == nil {
				return nil
			}
			// 跳过文件夹
			if f.IsDir() {
				return nil
			}
			// 判断文件是否存在
			if z.IsBlank(fname) {
				// 累加
				id++
				// 记录文件
				d.Files = append(d.Files,
						 &I{id, f.Name(),
						 fmt.Sprintf("%d", f.Size()), f.ModTime().String(), admin})
			} else {
				// 检查包含
				if strings.Contains(strings.ToLower(f.Name()), strings.ToLower(fname)) {
					// 累加
					id++
					// 记录文件
					d.Files = append(d.Files, 
							 &I{id, f.Name(),
							 fmt.Sprintf("%d", f.Size()), f.ModTime().String(), admin})
				}
			}
			// 返回
			return nil
		})

		// 解析主页面
		t, err := template.ParseFiles("template/files/filelist.html")
		if err != nil {
			// 输出错误信息
			http.Error(w, err.Error(), 500)
			return
		}

		// 执行
		t.Execute(w, d)

		// 返回
		return

	}
