---
title: Golang-字符串操作处理包-Strings
date: '2014-07-04'
description:
categories:

tags:golang

---

// 转自网络 <老虞学GoLang笔记-字符串>

在所有编程语言中都涉及到大量的字符串操作，可见熟悉对字符串的操作是何等重要。

Go中的字符串和C#中的一样，字符串内容在初始化后不可修改。

需要注意的是在Go中字符串是有UTF-8编码的，请注意保存文件时将文件编码格式改成UTF-8(特别是在windows下)。

>

初始化

>

	var str string //声明一个字符串
	str = "laoYu"  //赋值
	ch :=str[0]    //获取第一个字符
	len :=len(str) //字符串的长度,len是内置函数 ,len=5

>

字符串操作

>

码过程中避免不了中文字符，那我们该如何提取一个中文呢？

首先我们要知道string[index]获取的是字符byte

就无法像C#中"老虞"[0]来取到‘老’，在Go中需要将字符串转换成rune数组

runne数组中就可以通过数组下标获取一个汉字所标识的Unicode码，再将Unicode码按创建成字符串即可。

>	

	str :="laoYu老虞"

	for  i:=0;i<len(str);i++ {
		 fmt.Println(str[i])
	}

	for  i,s :=  range str {
		fmt.Println(i,"Unicode(",s,") string=",string(s))
	}

	r := []rune(str)
	fmt.Println("rune=",r)
	for i:=0;i<len(r) ; i++ {
	       fmt.Println("r[",i,"]=",r[i],"string=",string(r[i]))
	}

	Outut：
	108
	97
	111
	89
	117
	232
	128
	129
	232
	153
	158
	0 Unicode( 108 ) string= l
	1 Unicode( 97 ) string= a
	2 Unicode( 111 ) string= o
	3 Unicode( 89 ) string= Y
	4 Unicode( 117 ) string= u
	5 Unicode( 32769 ) string= 老
	8 Unicode( 34398 ) string= 虞
	rune= [108 97 111 89 117 32769 34398]
	r[ 0 ]= 108 string= l
	r[ 1 ]= 97 string= a
	r[ 2 ]= 111 string= o
	r[ 3 ]= 89 string= Y
	r[ 4 ]= 117 string= u
	r[ 5 ]= 32769 string= 老
	r[ 6 ]= 34398 string= 虞

>

对字符串的操作非常重要，来了解下strings包中提供了哪些函数

	获取总字节数 func Len(v type) int

len函数是Go中内置函数，不引入strings包即可使用。

	len(string)返回的是字符串的字节数。len函数所支持的入参类型如下：
	len(Array) 数组的元素个数
	len(*Array) 数组指针中的元素个数,如果入参为nil则返回0
	len(Slice) 数组切片中元素个数,如果入参为nil则返回0
	len(map) 字典中元素个数,如果入参为nil则返回0
	len(Channel) Channel buffer队列中元素个数

>

	str :="laoYu老虞"
	str2 :="laoYu"
	fmt.Println("len(",str,")=",len(str))      //len=11=5+6,一个汉字在UTF-8>中占3个字节
	fmt.Println("len(",str2,")=",len(str2))    //len=5
	fmt.Println("str[0]=",str[0])              //l

	str :="str"
	arr :=[5]int{1,2,3}
	slice :=make([]int,5)

	m :=make(map[int] string)
	m[2]="len"

	ch :=make(chan int)

	fmt.Println("len(string)=",len(str))   //3
	fmt.Println("len(array)=",len(arr))     //5invalid argument user (type *UserInfo) for len

	fmt.Println("len(slice)=",len(slice))   //5
	fmt.Println("len(map)=",len(m))         //1
	fmt.Println("len(chat)=",len(ch))       //0

	//user :=&UserInfo{id:1,name:"laoYu"}
	//interger :=2
	//fmt.Println("len(my struct)=",len(user))//invalid argument user (type *UserInfo) for len
	//fmt.Println("len(interger)=",len(interger))

	var str2 string
	var arr2  [5]int
	var slice2  []int
	var  m2 map[int] string
	var  ch2 chan int

	fmt.Println("len(string)=",len(str2))    //0
	fmt.Println("len(array)=",len(arr2))     //5 
	fmt.Println("len(slice)=",len(slice2))   //0
	fmt.Println("len(map)=",len(m2))         //0
	fmt.Println("len(chat)=",len(ch2))       //0

>

字符串中是否包含某字符串 func Contains(s, substr string) bool

确定是否包含某字符串，这是区分大小写的。

实际上内部是通过Index(s,sub string) int 实现的。如果索引!=-1则表示包含该字符串。

空字符串""在任何字符串中均存在。

	// Contains returns true if substr is within s.
	func Contains(s, substr string) bool {
	     return Index(s, substr) != -1
	}

>

	str :="laoYuStudyGotrue是否包含某字符串"
	fmt.Println(strings.Contains(str,"go"))         //false
	fmt.Println(strings.Contains(str,"Go"))         //true
	fmt.Println(strings.Contains(str,"laoyu"))      //false
	fmt.Println(strings.Contains(str,"是"))         //true
	fmt.Println(strings.Contains(str,""))           //true

>

在实际工作中常需要在不区分大小写的情况下确认是否包含某字符串

(我们应该减少这种情况，以免每次验证时都需要进行一次大小写转换)。

这里我局部修改源代码提供一个验证字符串中是否包含某字符串的函数

当然你也可以直接使用strings.Contains(strings.ToLower(s),strings.ToLower(substr))

>

	str := "laoYuStudyGotrue是否包含某字符串"
	fmt.Println(Contains(str, "go", true))  //true
	fmt.Println(Contains(str,"go",false))   //false

>

	//在字符串s中是否包含字符串substr,ignoreCase表示是否忽略大小写
	func Contains(s string, substr string, ignoreCase bool) bool {
	    return Index(s, substr, ignoreCase) != -1

	}

	//字符串subst在字符串s中的索引位置,ignoreCase表示是否忽略大小写
	func Index(s string, sep string, ignoreCase bool) int {

	    n := len(sep)
	    if n == 0 {
		    return 0
	    }

	    //to Lower
	    if ignoreCase == true {
		    s = strings.ToLower(s)
		    sep = strings.ToLower(sep)
	    }

	    c := sep[0]
	    if n == 1 {
		    // special case worth making fast
		    for i := 0; i < len(s); i++ {
			    if s[i] == c {
				    return i
			    }
		    }
		    return -1
	    }
	    // n > 1
	    for i := 0; i+n <= len(s); i++ {
		    if s[i] == c && s[i:i+n] == sep {
			    return i
		    }
	    }
	    return -1
	}

>

获取字符串sep在字符串s中出现的次数 Count(s,sep string)

注意：如果sep=""，则无论s为何字符串都会返回 len(s)+1

>

	fmt.Println(strings.Count("laoYuStudyGo", "o"))                 //2
	fmt.Println(strings.Count("laoYuStudyGo", "O"))                 //0
	fmt.Println(strings.Count("laoYuStudyGo", ""))                  //13=12+1
	fmt.Println(strings.Count("laoYuStudyGo老虞学习Go语言", "虞"))  //1
	fmt.Println(strings.Count("laoYuStudyGo老虞学习Go语言", "Go"))  //2
	fmt.Println(strings.Count("laoYuStudyGo老虞学习Go语言", "老虞"))//1
	fmt.Println(strings.Count("", ""))                             //1=0+1
	fmt.Println(strings.Count("aaaaaaaa","aa"))                     //4
	fmt.Println(strings.Count("laoYuStudyGo_n","\n"))               //0

>

**使用（多个）空格分割字符串 Fields(s string) ,返回分割后的数组 **

将字符串分割成数组，其分割符为空格。

>

	fmt.Println(strings.Fields("lao Yu Study Go ")) //OutPut: [lao Yu Study Go]
	fmt.Println(strings.Fields("   Go    "))        //[Go]
	fmt.Println(strings.Fields(""))                 //[]
	fmt.Println(strings.Fields(" \n go"))           //[go]

>

**其实其内部实现调用的是FieldsFunc(s,unicode.IsSpace),我们也可以自定义分割方式 **

>

	canSplit := func (c rune)  bool { return c=='#'}
	fmt.Println(strings.FieldsFunc("lao###Yu#Study####Go#G ",canSplit)) //[lao Yu Study Go G<space>]

>

检查字符串是否已某字符串开头 HasPrefix(s,prefix string) bool

如果想查看更多关于strings包下的字符串操作函数，请查看
		
---

	package main

	import (
	"fmt"
	"strings"
	//"unicode/utf8"
	)

	func main() {

		fmt.Println("查找子串是否在指定的字符串中")
		fmt.Println(" Contains 函数的用法")
		fmt.Println(strings.Contains("seafood", "foo")) //true
		fmt.Println(strings.Contains("seafood", "bar")) //false
		fmt.Println(strings.Contains("seafood", "")) //true
		fmt.Println(strings.Contains("", "")) //true 这里要特别注意
		fmt.Println(strings.Contains("我是中国人", "我")) //true

		fmt.Println("")
		fmt.Println(" ContainsAny 函数的用法")
		fmt.Println(strings.ContainsAny("team", "i")) // false
		fmt.Println(strings.ContainsAny("failure", "u & i")) // true
		fmt.Println(strings.ContainsAny("foo", "")) // false
		fmt.Println(strings.ContainsAny("", "")) // false

		fmt.Println("")
		fmt.Println(" ContainsRune 函数的用法")
		fmt.Println(strings.ContainsRune("我是中国", '我')) // true 注意第二个参数，用的是字符

		fmt.Println("")
		fmt.Println(" Count 函数的用法")
		fmt.Println(strings.Count("cheese", "e")) // 3 
		fmt.Println(strings.Count("five", "")) // before & after each rune result: 5 , 源码中有实现

		fmt.Println("")
		fmt.Println(" EqualFold 函数的用法")
		fmt.Println(strings.EqualFold("Go", "go")) //大小写忽略 

		fmt.Println("")
		fmt.Println(" Fields 函数的用法")
		fmt.Println("Fields are: %q", strings.Fields(" foo bar baz ")) //["foo" "bar" "baz"] 返回一个列表

		//相当于用函数做为参数，支持匿名函数
		for _, record := range []string{" aaa*1892*122", "aaa\taa\t", "124|939|22"} {
		fmt.Println(strings.FieldsFunc(record, func(ch rune) bool {
		switch {
		case ch > '5':
		return true
		}
		return false
		}))
		}

		fmt.Println("")
		fmt.Println(" HasPrefix 函数的用法")
		fmt.Println(strings.HasPrefix("NLT_abc", "NLT")) //前缀是以NLT开头的

		fmt.Println("")
		fmt.Println(" HasSuffix 函数的用法")
		fmt.Println(strings.HasSuffix("NLT_abc", "abc")) //后缀是以NLT开头的

		fmt.Println("")
		fmt.Println(" Index 函数的用法")
		fmt.Println(strings.Index("NLT_abc", "abc")) // 返回第一个匹配字符的位置，这里是4
		fmt.Println(strings.Index("NLT_abc", "aaa")) // 在存在返回 -1
		fmt.Println(strings.Index("我是中国人", "中")) // 在存在返回 6

		fmt.Println("")
		fmt.Println(" IndexAny 函数的用法")
		fmt.Println(strings.IndexAny("我是中国人", "中")) // 在存在返回 6
		fmt.Println(strings.IndexAny("我是中国人", "和")) // 在存在返回 -1

		fmt.Println("")
		fmt.Println(" Index 函数的用法")
		fmt.Println(strings.IndexRune("NLT_abc", 'b')) // 返回第一个匹配字符的位置，这里是4
		fmt.Println(strings.IndexRune("NLT_abc", 's')) // 在存在返回 -1
		fmt.Println(strings.IndexRune("我是中国人", '中')) // 在存在返回 6

		fmt.Println("")
		fmt.Println(" Join 函数的用法")
		s := []string{"foo", "bar", "baz"}
		fmt.Println(strings.Join(s, ", ")) // 返回字符串：foo, bar, baz 

		fmt.Println("")
		fmt.Println(" LastIndex 函数的用法")
		fmt.Println(strings.LastIndex("go gopher", "go")) // 3

		fmt.Println("")
		fmt.Println(" LastIndexAny 函数的用法")
		fmt.Println(strings.LastIndexAny("go gopher", "go")) // 4
		fmt.Println(strings.LastIndexAny("我是中国人", "中")) // 6

		fmt.Println("")
		fmt.Println(" Map 函数的用法")
		rot13 := func(r rune) rune {
		switch {
		case r >= 'A' && r <= 'Z':
		return 'A' + (r-'A'+13)%26
		case r >= 'a' && r <= 'z':
		return 'a' + (r-'a'+13)%26
		}
		return r
		}
		fmt.Println(strings.Map(rot13, "'Twas brillig and the slithy gopher..."))

		fmt.Println("")
		fmt.Println(" Repeat 函数的用法")
		fmt.Println("ba" + strings.Repeat("na", 2)) //banana 

		fmt.Println("")
		fmt.Println(" Replace 函数的用法")
		fmt.Println(strings.Replace("oink oink oink", "k", "ky", 2))
		fmt.Println(strings.Replace("oink oink oink", "oink", "moo", -1))

		fmt.Println("")
		fmt.Println(" Split 函数的用法")
		fmt.Printf("%q\n", strings.Split("a,b,c", ","))
		fmt.Printf("%q\n", strings.Split("a man a plan a canal panama", "a "))
		fmt.Printf("%q\n", strings.Split(" xyz ", ""))
		fmt.Printf("%q\n", strings.Split("", "Bernardo O'Higgins"))

		fmt.Println("")
		fmt.Println(" SplitAfter 函数的用法")
		fmt.Printf("%q\n", strings.SplitAfter("/home/m_ta/src", "/")) //["/" "home/" "m_ta/" "src"]

		fmt.Println("")
		fmt.Println(" SplitAfterN 函数的用法")
		fmt.Printf("%q\n", strings.SplitAfterN("/home/m_ta/src", "/", 2)) //["/" "home/m_ta/src"]
		fmt.Printf("%q\n", strings.SplitAfterN("#home#m_ta#src", "#", -1)) //["/" "home/" "m_ta/" "src"]

		fmt.Println("")
		fmt.Println(" SplitN 函数的用法")
		fmt.Printf("%q\n", strings.SplitN("/home/m_ta/src", "/", 1))

		fmt.Printf("%q\n", strings.SplitN("/home/m_ta/src", "/", 2)) //["/" "home/" "m_ta/" "src"]
		fmt.Printf("%q\n", strings.SplitN("/home/m_ta/src", "/", -1)) //["" "home" "m_ta" "src"]
		fmt.Printf("%q\n", strings.SplitN("home,m_ta,src", ",", 2)) //["/" "home/" "m_ta/" "src"]

		fmt.Printf("%q\n", strings.SplitN("#home#m_ta#src", "#", -1)) //["/" "home/" "m_ta/" "src"]

		fmt.Println("")
		fmt.Println(" Title 函数的用法") //这个函数，还真不知道有什么用
		fmt.Println(strings.Title("her royal highness"))

		fmt.Println("")
		fmt.Println(" ToLower 函数的用法")
		fmt.Println(strings.ToLower("Gopher")) //gopher 

		fmt.Println("")
		fmt.Println(" ToLowerSpecial 函数的用法")

		fmt.Println("")
		fmt.Println(" ToTitle 函数的用法")
		fmt.Println(strings.ToTitle("loud noises"))
		fmt.Println(strings.ToTitle("loud 中国"))

		fmt.Println("")
		fmt.Println(" Replace 函数的用法")
		fmt.Println(strings.Replace("ABAACEDF", "A", "a", 2)) // aBaACEDF
		//第四个参数小于0，表示所有的都替换， 可以看下golang的文档
		fmt.Println(strings.Replace("ABAACEDF", "A", "a", -1)) // aBaaCEDF

		fmt.Println("")
		fmt.Println(" ToUpper 函数的用法")
		fmt.Println(strings.ToUpper("Gopher")) //GOPHER

		fmt.Println("")
		fmt.Println(" Trim 函数的用法")
		fmt.Printf("[%q]", strings.Trim(" !!! Achtung !!! ", "! ")) // ["Achtung"]

		fmt.Println("")
		fmt.Println(" TrimLeft 函数的用法")
		fmt.Printf("[%q]", strings.TrimLeft(" !!! Achtung !!! ", "! ")) // ["Achtung !!! "]

		fmt.Println("")
		fmt.Println(" TrimSpace 函数的用法")
		fmt.Println(strings.TrimSpace(" \t\n a lone gopher \n\t\r\n")) // a lone gopher

	}

---------------------------------------------------

清理

	// 删除字符串前后的逗号
	arr := strings.Trim(val,",")   

搜索

	// 返回s中第一次出现sep的位置，如果没有返回-1.
	func  Index(s,  sep string)  int

	fmt.Println(strings.Index("chicken","ken"))
	fmt.Println(strings.Index("chicken","dmr"))

	func  LastIndex(s,  sep string)  int
	// 返回在s中最后一次出现sep的位置，不存在返回-1

	func  Count(s,  sep string)  int
	// 统计sep在s中出现的次数，非重叠的，比如s=“eeee”，sep=”ee”,结果返回

	fmt.Println(strings.Count("cheese","e"))
	fmt.Println(strings.Count("five",""))

包含

	func  Contains(s,  substr string)  bool
	// 如果substr在s中，返回true

	fmt.Println(strings.Contains("seafood","foo"))
	fmt.Println(strings.Contains("seafood","bar"))
	fmt.Println(strings.Contains("seafood",""))
	fmt.Println(strings.Contains("",""))

连接

	func  Join(a []string,  sep string)  string
	// 连接字符串，以sep作为分隔符

	s := []string{"foo","bar","baz"}
	fmt.Println(strings.Join(s,", "))

分割

	func  Split(s,  sep string)  []string
	// 分割字符串，sep为空字串时，与上一篇讲到的一样，相当于每个字符之间的间隔
	// s中没有sep，返回值里只有一个元素s

	fmt.Printf("%q\n",strings.Split("a,b,c",","))
	fmt.Printf("%q\n",strings.Split("a man a plan a canal panama","a "))
	fmt.Printf("%q\n",strings.Split(" xyz ",""))
	fmt.Printf("%q\n",strings.Split("","Bernardo O'Higgins"))

------------------------------

	package strings
	    import "strings"

	    Package strings implements simple functions to manipulate strings.

	FUNCTIONS

	func Contains(s, substr string) bool
	    Contains returns true if substr is within s.

	func ContainsAny(s, chars string) bool
	    ContainsAny returns true if any Unicode code points in chars are within
	    s.

	func ContainsRune(s string, r rune) bool
	    ContainsRune returns true if the Unicode code point r is within s.

	func Count(s, sep string) int
	    Count counts the number of non-overlapping instances of sep in s.

	func EqualFold(s, t string) bool
	    EqualFold reports whether s and t, interpreted as UTF-8 strings, are
	    equal under Unicode case-folding.

	func Fields(s string) []string
	    Fields splits the string s around each instance of one or more
	    consecutive white space characters, returning an array of substrings of
	    s or an empty list if s contains only white space.

	func FieldsFunc(s string, f func(rune) bool) []string
	    FieldsFunc splits the string s at each run of Unicode code points c
	    satisfying f(c) and returns an array of slices of s. If all code points
	    in s satisfy f(c) or the string is empty, an empty slice is returned.

	func HasPrefix(s, prefix string) bool
	    HasPrefix tests whether the string s begins with prefix.

	func HasSuffix(s, suffix string) bool
	    HasSuffix tests whether the string s ends with suffix.

	func Index(s, sep string) int
	    Index returns the index of the first instance of sep in s, or -1 if sep
	    is not present in s.

	func IndexAny(s, chars string) int
	    IndexAny returns the index of the first instance of any Unicode code
	    point from chars in s, or -1 if no Unicode code point from chars is
	    present in s.

	func IndexFunc(s string, f func(rune) bool) int
	    IndexFunc returns the index into s of the first Unicode code point
	    satisfying f(c), or -1 if none do.

	func IndexRune(s string, r rune) int
	    IndexRune returns the index of the first instance of the Unicode code
	    point r, or -1 if rune is not present in s.

	func Join(a []string, sep string) string
	    Join concatenates the elements of a to create a single string. The
	    separator string sep is placed between elements in the resulting string.

	func LastIndex(s, sep string) int
	    LastIndex returns the index of the last instance of sep in s, or -1 if
	    sep is not present in s.

	func LastIndexAny(s, chars string) int
	    LastIndexAny returns the index of the last instance of any Unicode code
	    point from chars in s, or -1 if no Unicode code point from chars is
	    present in s.

	func LastIndexFunc(s string, f func(rune) bool) int
	    LastIndexFunc returns the index into s of the last Unicode code point
	    satisfying f(c), or -1 if none do.

	func Map(mapping func(rune) rune, s string) string
	    Map returns a copy of the string s with all its characters modified
	    according to the mapping function. If mapping returns a negative value,
	    the character is dropped from the string with no replacement.

	func Repeat(s string, count int) string
	    Repeat returns a new string consisting of count copies of the string s.

	func Replace(s, old, new string, n int) string
	    Replace returns a copy of the string s with the first n non-overlapping
	    instances of old replaced by new. If n < 0, there is no limit on the
	    number of replacements.

	func Split(s, sep string) []string
	    Split slices s into all substrings separated by sep and returns a slice
	    of the substrings between those separators. If sep is empty, Split
	    splits after each UTF-8 sequence. It is equivalent to SplitN with a
	    count of -1.

	func SplitAfter(s, sep string) []string
	    SplitAfter slices s into all substrings after each instance of sep and
	    returns a slice of those substrings. If sep is empty, SplitAfter splits
	    after each UTF-8 sequence. It is equivalent to SplitAfterN with a count
	    of -1.

	func SplitAfterN(s, sep string, n int) []string
	    SplitAfterN slices s into substrings after each instance of sep and
	    returns a slice of those substrings. If sep is empty, SplitAfterN splits
	    after each UTF-8 sequence. The count determines the number of substrings
	    to return:

		n > 0: at most n substrings; the last substring will be the unsplit remainder.
		n == 0: the result is nil (zero substrings)
		n < 0: all substrings

	func SplitN(s, sep string, n int) []string
	    SplitN slices s into substrings separated by sep and returns a slice of
	    the substrings between those separators. If sep is empty, SplitN splits
	    after each UTF-8 sequence. The count determines the number of substrings
	    to return:

		n > 0: at most n substrings; the last substring will be the unsplit remainder.
		n == 0: the result is nil (zero substrings)
		n < 0: all substrings

	func Title(s string) string
	    Title returns a copy of the string s with all Unicode letters that begin
	    words mapped to their title case.

	func ToLower(s string) string
	    ToLower returns a copy of the string s with all Unicode letters mapped
	    to their lower case.

	func ToLowerSpecial(_case unicode.SpecialCase, s string) string
	    ToLowerSpecial returns a copy of the string s with all Unicode letters
	    mapped to their lower case, giving priority to the special casing rules.

	func ToTitle(s string) string
	    ToTitle returns a copy of the string s with all Unicode letters mapped
	    to their title case.

	func ToTitleSpecial(_case unicode.SpecialCase, s string) string
	    ToTitleSpecial returns a copy of the string s with all Unicode letters
	    mapped to their title case, giving priority to the special casing rules.

	func ToUpper(s string) string
	    ToUpper returns a copy of the string s with all Unicode letters mapped
	    to their upper case.

	func ToUpperSpecial(_case unicode.SpecialCase, s string) string
	    ToUpperSpecial returns a copy of the string s with all Unicode letters
	    mapped to their upper case, giving priority to the special casing rules.

	func Trim(s string, cutset string) string
	    Trim returns a slice of the string s with all leading and trailing
	    Unicode code points contained in cutset removed.

	func TrimFunc(s string, f func(rune) bool) string
	    TrimFunc returns a slice of the string s with all leading and trailing
	    Unicode code points c satisfying f(c) removed.

	func TrimLeft(s string, cutset string) string
	    TrimLeft returns a slice of the string s with all leading Unicode code
	    points contained in cutset removed.

	func TrimLeftFunc(s string, f func(rune) bool) string
	    TrimLeftFunc returns a slice of the string s with all leading Unicode
	    code points c satisfying f(c) removed.

	func TrimRight(s string, cutset string) string
	    TrimRight returns a slice of the string s, with all trailing Unicode
	    code points contained in cutset removed.

	func TrimRightFunc(s string, f func(rune) bool) string
	    TrimRightFunc returns a slice of the string s with all trailing Unicode
	    code points c satisfying f(c) removed.

	func TrimSpace(s string) string
	    TrimSpace returns a slice of the string s, with all leading and trailing
	    white space removed, as defined by Unicode.
