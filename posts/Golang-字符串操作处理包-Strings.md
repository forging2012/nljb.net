---
title: Golang-字符串操作处理包-Strings
date: '2014-07-04'
description:
categories:

tags:golang

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
