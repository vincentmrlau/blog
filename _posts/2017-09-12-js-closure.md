---
title:  javaScipt的闭包
date:   2017-09-12 17:50:18
categories:
- javascript
tags:
- javascript
excerpt: 很久之前在stackoverflow看到的一个关于闭包的答案，翻译一下。
published: false
---

# 一个闭包的例子

两句话总结：

* 闭包是一种支持`first-class functions`的方式；是变量在声明之后可以在作用域之内使用，可以被分配给变量，可以被作为参数传递给函数，可以作为函数的返回值的一种解释。
* 或则，闭包是指当函数执行完返回之后，函数开始执行被分配的堆结构没有被回收。

下面的例子返回一个函数的引用：

```javaScript
function sayHello2(name) {
  var text = 'Hello ' + name; // Local variable
  var say = function() { console.log(text); }
  return say;
}
var say2 = sayHello2('Bob');
say2(); // logs "Hello Bob"
```

大多数的js程序员都明白，上面的代码是如何把一个函数的引用返回到一个变量（`say2`）中的。如果你不明白，那么你需要在学习闭包之前重新看一下这部分的内容。C语言的程序员会考虑，这个函数返回的指向另一个函数的指针，`say`和`say2`是指向同一个函数的指针。

这是C语言的函数指针和js对函数的引用的一个重要的不同。在js中，你可以想想的是，一个函数以指针和隐藏的指向闭包的指针的方式引用变量。

上面的代码中存在闭包，因为我们在一个函数里面声明了函数
