---
title: Golang-GUI-客户端
date: '2014-09-04'
description:
categories:

tags:golang

---

自己写了个例子，学习学习

---

	package main

	import (
		"github.com/andlabs/ui"
		//z "github.com/nutzam/zgo"
		//"io/ioutil"
		//"net/http"
		//"net/url"
		//"os"
		"fmt"
		"reflect"
		//"regexp"
		"encoding/json"
		"log"
		"runtime"
		"time"
	)

	var (
		Status = false
		Server = "127.0.0.1:8080"
	)

	func main() {

		// 设置CPU核心数量
		runtime.GOMAXPROCS(runtime.NumCPU())

		// 设置日志的结构
		log.SetFlags(log.Lshortfile | log.Ldate | log.Ltime | log.Lmicroseconds)

		go ui.Do(initGUI)

		err := ui.Go()
		if err != nil {
			panic(err)
		}

	}

	func initGUI() {

		// ------------------------------------------- //

		//// 创建一个标签
		//serLable := ui.NewLabel("服务器:")

		//// 创建一个文本输入区域
		//serAddr = ui.NewTextField()

		//// 设置文本输入区域事件
		//serAddr.OnChanged(func() {
		//	go feedback()
		//})

		//// 创建一个按钮
		//connButton := ui.NewButton("连接")

		//// 设置按钮事件
		//connButton.OnClicked(guiTranslate)

		//// 设置为横向排列结构
		//serAddr := ui.NewHorizontalStack(serLable, serAddr, connButton)

		//// 设置第二个对象弹性填充
		//serAddr.SetStretchy(1)

		// ------------------------------------------- //

		// 系统
		sysTable := ui.NewTable(reflect.TypeOf(Sys{}))

		// 系统更新线程
		go (func() {

			for {

				// ------------------------- //
				sysTable.Lock()
				d := sysTable.Data().(*[]Sys)
				*d = []Sys{}
				sysTable.Unlock()
				// ------------------------- //

				var systems Systems

				// 获取系统信息
				sysData, err := Request(fmt.Sprintf("http://%s/wsys", Server))
				if err != nil {
					Status = false
					log.Println(err)
					time.Sleep(1 * time.Second)
					continue
				} else {
					Status = true
				}

				err = json.Unmarshal(*sysData, &systems)
				if err != nil {
					log.Println(err)
					time.Sleep(1 * time.Second)
					continue
				}

				s := make([]Sys, len(systems.System))
				for i, systems := range systems.System {
					s[i].Name = (*systems).Name
					s[i].Sysboot = (*systems).Sysboot
					s[i].System = (*systems).System
					s[i].Remote_command = (*systems).Remote_command
				}

				// ------------------------- //
				sysTable.Lock()
				d = sysTable.Data().(*[]Sys)
				*d = s
				sysTable.Unlock()
				// ------------------------- //

				time.Sleep(1 * time.Second)

			}

		})()

		// ------------------------------------------- //

		// 创建一组系统数据列表
		evos := make([]Evo, 0)

		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})
		evos = append(evos, Evo{"1", "2", "3"})

		// 创建表,并导入系统数据
		evoTable := ui.NewTable(reflect.TypeOf(evos[0]))
		evoTable.Lock()
		*(evoTable.Data().(*[]Evo)) = evos
		evoTable.Unlock()

		// ------------------------------------------- //

		// 创建一组系统数据列表
		mbrs := make([]Mbr, 0)
		mbrs = append(mbrs, Mbr{"1"})
		mbrs = append(mbrs, Mbr{"1"})
		mbrs = append(mbrs, Mbr{"1"})

		// 创建表,并导入系统数据
		mbrTable := ui.NewTable(reflect.TypeOf(mbrs[0]))
		mbrTable.Lock()
		*(mbrTable.Data().(*[]Mbr)) = mbrs
		mbrTable.Unlock()

		// ------------------------------------------- //

		// 创建一组系统数据列表
		patchs := make([]Patch, 0)
		patchs = append(patchs, Patch{"1"})
		patchs = append(patchs, Patch{"1"})
		patchs = append(patchs, Patch{"1"})

		// 创建表,并导入系统数据
		patchTable := ui.NewTable(reflect.TypeOf(patchs[0]))
		patchTable.Lock()
		*(patchTable.Data().(*[]Patch)) = patchs
		patchTable.Unlock()

		// ------------------------------------------- //

		// 创建一组系统数据列表
		runs := make([]Run, 0)
		runs = append(runs, Run{"1", "2", "3", "4", "5"})
		runs = append(runs, Run{"1", "2", "3", "4", "5"})
		runs = append(runs, Run{"1", "2", "3", "4", "5"})
		runs = append(runs, Run{"1", "2", "3", "4", "5"})

		// 创建表,并导入系统数据
		runTable := ui.NewTable(reflect.TypeOf(runs[0]))
		runTable.Lock()
		*(runTable.Data().(*[]Run)) = runs
		runTable.Unlock()

		// ------------------------------------------- //

		// 创建一个标签
		tab := ui.NewTab()
		// 将表加入标签
		tab.Append("系统", sysTable)
		// 将表加入标签
		tab.Append("进化", evoTable)
		// 将表加入标签
		tab.Append("分区表", mbrTable)
		// 将表加入标签
		tab.Append("补丁", patchTable)
		// 将表加入标签
		tab.Append("运行", runTable)

		// ------------------------------------------- //

		// 垂直堆叠
		stack := ui.NewVerticalStack(tab)

		// 弹性填充
		stack.SetStretchy(0)

		// 布局窗口
		w := ui.NewWindow("北京德纳科技有限公司", 800, 400, stack)
		w.OnClosing(func() bool {
			ui.Stop()
			return true
		})
		go (func() {
			for {
				if Status {
					w.SetTitle("北京德纳科技有限公司(在线)")
				} else {
					w.SetTitle("北京德纳科技有限公司(离线)")
				}
				w.Show()
				time.Sleep(1 * time.Second)
			}
		})()

	}


