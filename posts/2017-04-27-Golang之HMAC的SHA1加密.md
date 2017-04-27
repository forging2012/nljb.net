---
title: Golang之HMAC的SHA1加密
date: '2017-04-27'
description:
categories:

tags:golang

---

>

#### Golang之HMAC的SHA1加密

>


	HMAC是密钥相关的哈希运算消息认证码（Hash-based Message Authentication Code）
	HMAC运算利用哈希算法，以一个密钥和一个消息为输入，生成一个消息摘要作为输出
	// 也就是说HMAC通过将哈希算法(SHA1, MD5)与密钥进行计算生成摘要...可解密...

>

	// 在 PHP 中是 hash_hmac('sha1',$string,$key);

	package main

	import (
		"crypto/hmac"
		"crypto/sha1"
		"fmt"
		"io"
	)

	func main() {
		//sha1
		h := sha1.New()
		io.WriteString(h, "aaaaaa")
		fmt.Printf("%x\n", h.Sum(nil))

		//hmac ,use sha1
		key := []byte("123456")
		mac := hmac.New(sha1.New, key)
		// mac := hmac.New(md5.New, key)
		mac.Write([]byte("aaaaaa"))
		fmt.Printf("%x\n", mac.Sum(nil))
	}

>

	安全哈希算法（Secure Hash Algorithm）
	主要适用于数字签名标准 （Digital Signature Standard DSS）里面定义的数字签名算法（Digital Signature Algorithm DSA）。
	对于长度小于2^64位的消息，SHA1会产生一个160位的消息摘要。
	当接收到消息的时候，这个消息摘要可以用来验证数据的完整性。
	在传输的过程中，数据很可能会发生变化，那么这时候就会产生不同的消息摘要。
	SHA1有如下特性：不可以从消息摘要中复原信息；两个不同的消息不会产生同样的消息摘要,(但会有1x10 ^ 48分之一的机率出现相同的消息摘要,一般使用时忽略)。

>
