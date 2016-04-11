---
title: Go-SSH
date: '2014-11-07'
description:
categories:

tags:golang

---

	package main

	import (
		"bufio"
		"fmt"
		"os"

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
			return nil, fmt.Errorf(fmt.Sprintf("Connect fail \n(%s)", addr, err.Error()))
		}

		return client, nil
	}

	func (this *Node) Command(client *ssh.Client, command string) {
		session, err := client.NewSession()
		if err != nil {
			fmt.Println(err)
			return
		}
		defer session.Close()
		output, err := session.Output(command)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Println(string(output))
	}

	func main() {

		node := NewNode("root", "...")

		client, err := node.Conn("192.168.0.100")
		if err != nil {
			fmt.Println(err)
			return
		}
		defer client.Close()

		input := bufio.NewReader(os.Stdin)
		for {
			fmt.Print("$: ")
			line, _ := input.ReadString('\n')
			if Trim(line) == "bye" {
				os.Exit(0)
			}
			node.Command(client, Trim(line))
		}

		return
	}

	func IsSpace(c byte) bool {
		if c >= 0x00 && c <= 0x20 {
			return true
		}
		return false
	}

	func Trim(s string) string {
		size := len(s)
		if size <= 0 {
			return s
		}
		l := 0
		for ; l < size; l++ {
			b := s[l]
			if !IsSpace(b) {
				break
			}
		}
		r := size - 1
		for ; r >= l; r-- {
			b := s[r]
			if !IsSpace(b) {
				break
			}
		}
		return string(s[l : r+1])
	}


