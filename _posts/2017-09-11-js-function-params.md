---
title:  JS函数的传参传的是什么？
date:   2017-09-11 17:50:18
tags:
- javascript

excerpt: 最近在要就python,在函数的内部修改该参数的时候，可能会影响外部的使用，类似C++的值传递和引用传递，JS应该也会有类似的情况。
published: false
---
# code

```javaScript
var a = {a: 'hh'}
	c = 1,
	d = ['1','2'],
	e = {e:'22'}
function changeA(a, c, d, e) {
	a.a = 'hehe'
	c = 2
	d[1] = 3
	// 重新赋值，跟 c 的情况是一样的，用另外的地址存放变量了
	e = {d:33}
}

changeA(a, c, d)

console.log(a) // {b: 'hehe'}
console.log(c) // 1
console.log(d) // [1, 3]
console.log(e) // {e: '22'}
```
