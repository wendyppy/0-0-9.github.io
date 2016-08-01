---
layout: post
title: "JavaScript中的变量提升"
keywords: ["JavaScript","变量提升","js","var"]
description: "详细解读JavaScript中的变量提升"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

##函数级作用域

ES6之前，JavaScript 没有块级作用域。一个独立的执行环境不是由封闭的花括号而是函数来分界的。

<pre>
if(true){
    var girl = "Selina";
}
alert(girl);	//"Selina"
</pre>

这段代码中，执行`alert(girl);`没有返回`Uncaught ReferenceError: girl is not defined`这样的异常，而是弹出 if 语句中定义的"Selina"，这是为什么呢？

if 语句是一个由花括号封闭起来的“块”，在 Java、C 等语言中，块中定义的局部变量会在 if 语句执行完毕后被销毁，而在 JavaScript 中，if 语句中的变量声明会将变量添加到当前的执行环境。

for 语句同理。

<pre>
for(var i = 1; i < 10; i++){
    //do something...
}
alert(i);	//10
</pre>

如果你想像在 Java 中那样使用块级作用域，你可以用 ES6 新增的 let 声明块级作用域的变量，const 声明常量。不过这不是我们今天主要讨论的范围，暂不多做介绍。

##变量提升

熟悉 C 类语法的朋友应该知道，C、Java 这类语言中的变量必须“先声明，后使用”，否则就会报错。而 JavaScript 却能够在变量和函数声明之前就使用。

JavaScript 中，变量有4种方式进入作用域：

1. 语言内置。执行环境中的 this 和 arguments。（全局环境中无 arguments 对象）
2. 形式参数。函数的形参会作为作用域的一部分。
3. 函数声明。形如`function showGirl(){}`的形式。
4. 变量声明。形如`var girl`。

只有第三点提到的函数声明和第四点中 var 形式的变量声明才能进行提升。

提升，可以按照字面意思理解，就是把函数或变量声明在解析时提到执行环境最前面。

<pre>
name = "Selina";
var name;
alert(name);	//"Selina"
</pre>

JavaScript 引擎解析代码时其实把上面的代码变成：

<pre>
var name;
name = "Selina";
alert(name);	//"Selina"
</pre>

变量 name 的声明被提到了最前面，所以 alert 函数能弹出`Selina`而不是`undefined`。

再强调一下，只有用 var 声明的变量才存在变量提升，用 let、const 进行声明并不会提升。



