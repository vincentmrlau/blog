---
title:  js的变量提升（hoisting）
date:   2017-09-11 17:46:18
categories:
- javascript
tags:
- javascript
excerpt: Hoisting 被认为是思考执行上下文（特别是创建和执行阶段）在JavaScript中如何工作的一般方式。但是，hoisting 可能会导致误解。例如，提升教导变量和函数声明被物理移动到编码的顶部，但这根本不是什么。真正发生的什么是在编译阶段将变量和函数声明放入内存中，但仍然保留在编码中键入的位置。 -- mdn
published: true
---
# 前言
> “Hoisting 被认为是思考执行上下文（特别是创建和执行阶段）在JavaScript中如何工作的一般方式。但是，hoisting 可能会导致误解。例如，提升教导变量和函数声明被物理移动到编码的顶部，但这根本不是什么。**真正发生的什么是在编译阶段将变量和函数声明放入内存中，但仍然保留在编码中键入的位置。**” --[mdn](https://developer.mozilla.org/zh-CN/docs/Glossary/Hoisting)
> 
> 变量可以在声明之前进行初始化和使用

# 表现
## function
```javaScript
console.log(showName) // Function
// 调用方法
showName('vincent') //log vincent
function showName(name) {
	console.log(name)
}

// console.log(showName2) //抛出一个错误，showName2 is not defined
```

## var
```javaScript
console.log(memeda)//undefined
memeda = 2
console.log(memeda) // 2
var memeda

console.log(memeda2) // undefined
var memeda2 = 123


console.log(showHaha) // undefined
// 调用方法
// showHaha() // 抛出一个错误，showHaha is not a function
// 这样写不会提升
var showHaha = function () {
	console.log('haha')
}
```

## let const
> `let`和`const`可不随便提升

```javaScipt
console.log(memedalet)//抛出一个错误
let memedalet

console.log(_const) //抛出一个错误
const _const ='HHH'
```

## function 和 var 的提升级别
> 使用`var`和`function`重复声明时，先声明的后提升 

```javaScript
console.log('_varFunction', _varFunction) // undefined
var _varFunction = 2
function _varFunction2() {
	console.log('_varFuntion')
}

console.log('_varFunction2', _varFunction2) // function
function _varFunction2() {
	console.log('_varFuntion')
}
var _varFunction2 = 2
```