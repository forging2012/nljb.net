---
title: Go-SSH-使用
date: '2014-11-26'
description:
categories:

tags:golang

---

### 建立连接 (密码登陆模式)

>

	package main

	import (
		"fmt"

		"code.google.com/p/go.crypto/ssh"
	)

	type Node struct {
		User     string
		Password string
	}

	func NewNode(user, password string) *Node {
		node := new(Node)
		node.User = user
		node.Password = password
		return node
	}

	func (this *Node) Conn(addr string) (*ssh.Client, error) {

		authMethods := []ssh.AuthMethod{}

		keyboardInteractiveChallenge := func(
			user,
			instruction string,
			questions []string,
			echos []bool,
		) (answers []string, err error) {
			if len(questions) == 0 {
				return []string{}, nil
			}
			return []string{this.Password}, nil
		}

		authMethods = append(authMethods, ssh.KeyboardInteractive(keyboardInteractiveChallenge))
		authMethods = append(authMethods, ssh.Password(this.Password))

		sshConfig := &ssh.ClientConfig{
			User: this.User,
			Auth: authMethods,
		}

		client, err := ssh.Dial("tcp", fmt.Sprintf("%s:22", addr), sshConfig)
		if err != nil {
			return nil, err
		}

		return client, nil
	}


---

### 建立 Session 并设置

>

	// Root
	node := NewNode("root", "123456")

	// Connect
	client, err := node.Conn("127.0.0.1")
	if err != nil {
		panic(err)
	}
	defer client.Close()

	session, err := client.NewSession()
	if err != nil {
		panic(err)
	}
	defer session.Close()

	// 注意：如果想模拟SSH，可以将流输出到os.Stdout和os.Stderr中.
	// Set IO
	stdin, _ := session.StdinPipe()
	stdout, _ := session.StdoutPipe()

	// 以下设置，可以支持目录，文件，颜色输出
	/*

		// Set up terminal modes
		modes := ssh.TerminalModes{
			ssh.ECHO:          0,     // disable echoing
			ssh.TTY_OP_ISPEED: 14400, // input speed = 14.4kbaud
			ssh.TTY_OP_OSPEED: 14400, // output speed = 14.4kbaud
		}

		// Request pseudo terminal
		if err := session.RequestPty("xterm", 80, 40, modes); err != nil {
			panic(err)
		}

	*/

	// Start remote shell
	if err := session.Shell(); err != nil {
		panic(err)
	}

---

### 使用

>

	// 执行
	stdin.Write([]byte(fmt.Sprintf("%s 2>&1; echo -e End-Goline-End\n", command)))

	// 读取返回
	r_ := bufio.NewReader(stdout)

	//  返回数据
	datas := make([]byte, 0)

	// 读取客户端返回
	for {
		data, err := r_.ReadBytes('\n')
		if err != nil {
			break
		}
		if z.Trim(string(data)) == "End-Goline-End" {
			// 返回
			fmt.Println(z.Trim(string(datas))) 
			// 清零
			datas = []byte{}
			// 跳出
			break
		}
	}
