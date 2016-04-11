---
title: Golang之编码的使用
date: '2015-06-25'
description:
categories:

tags:golang

---

>

### AES 与 DEA 高级加密解密方法

>

	crypto/aes 是美国联邦政府采用的一种区块加密标准
	crypto/des 一种对称加密算法，是目前使用最广泛的密钥系统
				特别是在保护金融数据安全中...

>

	// AES 与 DES 实现方法类似, 下面是 AES 介绍

	package main

	import (
		"crypto/aes"
		"crypto/cipher"
		"fmt"
	)

	// 数据操作的偏移量
	var IV = []byte{0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0a, 0x0b, 0x0c, 0x0d, 0x0e, 0x0f}

	func main() {

		// 要加密的内容
		content := []byte("my name is nljb")

		// KEY, 必须是16,24,32位的[]byte
		// 分别对应AES_128,AES_192,AES_256
		key := "B8XKCA7IVW6WB7GX76V771RN8LJCY2H0"		

		// 通过密钥生成一个新的密码块
		c, err := aes.NewCipher([]byte(key))
		if err != nil {
			panic(err)
		}

		// 加密模式 (ECB、CBC、CFB、OFB)
		// IV是initialization vector的意思
		// 就是加密的初始话矢量，初始化加密函数的变量
		// 也就是加密动作中的 数据操作的偏移量
		cfb := cipher.NewCFBEncrypter(c, IV)

		// 存储密码, 必须与块体的长度相同
		ciphertext := make([]byte, len(content))

		// 流化, 必须与块体的长度相同
		cfb.XORKeyStream(ciphertext, content)

		// 输出
		fmt.Println(string(content), ciphertext)

		// ------------------------------------- //

		// 解密模式 (ECB、CBC、CFB、OFB)
		// 也就是说，解密的时候也需要加密时的密钥与偏移量
		cfbdec := cipher.NewCFBDecrypter(c, IV)

		// 存储数据, 必须与块体的长度相同
		plaintextCopy := make([]byte, len(content))

		// 流化, 必须与块体的长度相同
		cfbdec.XORKeyStream(plaintextCopy, ciphertext)

		// 输出
		fmt.Println(ciphertext, string(plaintextCopy))

	}

---

>

### Base64/Base32编码

>

	是网络上最常见的用于传输8Bit字节代码的编码方式之一，大家可以查看RFC2045～RFC2049，上面有MIME的详细规范。
	Base64编码可用于在HTTP环境下传递较长的标识信息。例如在Java Persistence系统Hibernate中
	就采用了Base64来将一个较长的唯一标识符（一般为128-bit的UUID）编码为一个字符串用作HTTP表单和HTTP GET URL中的参数。
	在其他应用程序中，也常常需要把二进制数据编码为适合放在URL（包括隐藏表单域）中的形式。
	此时，采用Base64编码具有不可读性，即所编码的数据不会被人用肉眼所直接看到。

>

	Base64编码说明
	Base64编码要求把3个8位字节(3*8=24)转化为4个6位的字节(4*6=24)之后在6位的前面补两个0,形成8位一个字节的形式。
	如果剩下的字符不足3个字节,则用0填充,输出字符使用'=',因此编码后输出的文本末尾可能会出现1或2个'='。
	为了保证所输出的编码位可读字符,Base64制定了一个编码表,以便进行统一转换.编码表的大小为2^6=64,这也是Base64名称的由来。

>

	0	A	16	Q	32	g	48	w
	1	B	17	R	33	h	49	x
	2	C	18	S	34	i	50	y
	3	D	19	T	35	j	51	z
	4	E	20	U	36	k	52	0
	5	F	21	V	37	l	53	1
	6	G	22	W	38	m	54	2
	7	H	23	X	39	n	55	3
	8	I	24	Y	40	o	56	4
	9	J	25	Z	41	p	57	5
	10	K	26	a	42	q	58	6
	11	L	27	b	43	r	59	7
	12	M	28	c	44	s	60	8
	13	N	29	d	45	t	61	9
	14	O	30	e	46	u	62	+
	15	P	31	f	47	v	63	/

>

	编码内容: `"'www.baidu.com','www.sina.com'"`,!@#$%^&*(){}[]?<>.
	编码结果: YCInd3d3LmJhaWR1LmNvbScsJ3d3dy5zaW5hLmNvbSciYCwhQCMkJV4mKigpe31bXT88Pi4=

>

	package main

	import (
		// "encoding/base32"
		"encoding/base64"
		"fmt"
	)

	func base64Encode(src []byte) []byte {
		return []byte(base64.StdEncoding.EncodeToString(src))
	}

	func base64Decode(src []byte) ([]byte, error) {
		return base64.StdEncoding.DecodeString(string(src))
	}

	func main() {

		nljb := "www.nljb.net"
		debyte := base64Encode([]byte(nljb))
		fmt.Println(string(debyte))

		srbyte, err := base64Decode(debyte)
		fmt.Println(string(srbyte), err)

	}

	// 输出
	d3d3Lm5samIubmV0
	www.nljb.net <nil>


---

>

### Gob编码

>

	gob是Golang包自带的一个数据结构序列化的编码/解码工具。
	编码使用Encoder，解码使用Decoder。
	一种典型的应用场景就是RPC(remote procedure calls)。
	gob和json的pack之类的方法一样，由发送端使用Encoder对数据结构进行编码。
	在接收端收到消息之后，接收端使用Decoder将序列化的数据变化成本地变量。

>

	package main
	 
	import (
		"bytes"
		"encoding/gob"
		"fmt"
		"log"
	)
	  
	type P struct {
		X, Y, Z int
		Name    string
	}
	   
	type Q struct {
		X, Y *int32
		Name string
	}
		
	func main() {

		var network bytes.Buffer       
		enc := gob.NewEncoder(&network)
		dec := gob.NewDecoder(&network)
		// Encode (send) the value.
		err := enc.Encode(P{3, 4, 5, "Pythagoras"})
		if err != nil {
			log.Fatal("encode error:", err)
		}
		// Decode (receive) the value.
		var q Q
		err = dec.Decode(&q)
		if err != nil {
			log.Fatal("decode error:", err)
		}
		fmt.Println(q)
		fmt.Printf("%q: {%d,%d}\n", q.Name, *q.X, *q.Y)

	}
																						 
>

---

>

### UrlEncode编码

>

	将字符串以 URL 编码

>

	编码内容: http://www.baidu.com/p/home/index.html
	编码结果: http%3a%2f%2fwww.baidu.com%2fp%2fhome%2findex.html

>

---

>

### Hex编码/十六进制编码

>

	将字符串以 十六进制 编码

>

	编码内容: www.baidu.com
	编码结果: 7777772e62616964752e636f6d20

>

---

>

	...
