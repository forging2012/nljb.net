---
title: Golang之如何安全的存储密码
date: '2015-07-23'
description:
categories:

tags:golang

---

>

### 如何安全的存储密码

>

	过去一段时间以来，许多网站遭遇用户密码泄漏事件，诸多社区有可能成为
	下一个目标，层出不穷的类似事件给用户的网上生活造成巨大影响人人自危
	因为人们往往习惯不同网站使用相同密码，所以一家暴露，全部遭殃 ...

	那么如何选择用户密码存储方案，才能避免这些陷阱呢 ...

>

---

>

### 单向哈希

>

	目前用得最多的密码存储方案是将明文密码做单向哈希后存储，单向哈希算法有一个特征：
		无法通过哈希后的摘要(digest)恢复原始数据，这也是单向二字的来源

>

	常用的单向哈希算法包括 SHA-256, SHA-1, MD5 等

>

---

>

	Go语言对这三种加密算法的实现如下:

	package main

	import (
		"crypto/md5"
		"crypto/sha1"
		"crypto/sha256"
		"fmt"
		"io"
	)

	func main() {

		// crypto/sha256
		a := sha256.New()
		io.WriteString(a, "www.nljb.net")
		fmt.Printf("%x\n", a.Sum(nil))

		// crypto/sha1
		b := sha1.New()
		io.WriteString(b, "www.nljb.net")
		fmt.Printf("%x\n", b.Sum(nil))

		// crypto/md5
		c := md5.New()
		io.WriteString(c, "www.nljb.net")
		fmt.Printf("%x\n", c.Sum(nil))

	}

	// 输出
	2ff738c6cd2bc6fc76c6b75a5caa5edccef3aca2803fa9838ad75d61aa06b6f6
	fa26f560a87db90e666f8b3c6e5df413e3370b89
	d30f05c05f1b6b6000bd6ce450348fc2

>

---

>

    // 单向哈希有两个特性：
    1，同一个密码进行单向哈希，得到的总是唯一确定的摘要
    2，计算速度快，随着技术进步，一秒钟能够完成数十亿次单向哈希计算

>

---

>

    // 随之而来的问题
    结合上面两个特点，考虑到多数人所使用的密码为常见的组合，攻击者可以将所有
    密码的常见组合进行单向哈希，得到一个摘要组合，然后与数据库中的摘要进行对比
    可以获得对应密码，这个摘要组合也被称为，rainbow table
    因此通过单向加密之后存储的数据，和明文存储没有多大区别, 因此一旦网站的数据库泄露
    所有用户的密码就大白于天下

>

    // 解决 rainbow table 问题的方案
    现在比较好的网站，都会用一种叫做加盐的方式来存储密码，也就是常说的salt
    他们通常的做法是先将用户输入的密码进行一次MD5加密，然后在MD5值前后加上
    一些只有管理员自己知道的随机串，再进行一次MD5加密，这个随机串中可以包括
    某些固定的串，比如用户名（用来保证每个用户加密使用的密钥都不一样)

>

	package main

	import (
		"bytes"
		"crypto/md5"
		"fmt"
		"io"
	)

	func main() {

		// 计算密码MD5
		c := md5.New()
		io.WriteString(c, "www.nljb.net")
		spw := fmt.Sprintf("%x\n", c.Sum(nil))

		// 指定两个(salt)
		salt1 := "@#$%"
		salt2 := "^&*()"

		// 拼接密码MD5
		buf := bytes.NewBufferString("")

		// 拼接密码
		io.WriteString(buf, salt1)
		io.WriteString(buf, spw)
		io.WriteString(buf, salt2)

		// 拼接密码计算MD5
		t := md5.New()
		io.WriteString(t, buf.String())

		// 输出
		fmt.Printf("%x\n", t.Sum(nil))

	}

>

---

>

	// 针对 rainbow table 这一类方案有一个特点
	算法中都有个因子，用于指明计算密码摘要所需要的资源和时间
	也就是计算强度，计算强度越大，攻击者建立rainbow table越困难
	以至于不可继续

>

	这里推荐scrypt方案，scrypt是由著名的FerrBSD黑客Colin Percival
	为他的备份服务Tarsnap开发的.

>

	// 获取地址
	https://code.google.com/p/go/source/browse/scrypt?repo=crypto

	// 该方法可以获取唯一的相应密码值，这是目前为止最难破解的
	dk := scrypt.Key([]byte("www.nljb.net"), []byte(salt), 16384, 8, 1, 32)

