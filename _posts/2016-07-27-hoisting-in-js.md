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

JavaScript 中，变量有4种方式进入作用域：（这也是变量解析的顺序）

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

变量 name 的声明被提到了最前面，所以 alert 函数能弹出`Selina`而不是`undefined`。注意，变量的赋值没有被提升，提升的只有变量的声明。

再强调一下，只有用 var 声明的变量才存在变量提升，用 let、const 进行声明并不会提升。

##函数提升

###函数声明与函数表达式

我们在创建函数的时候，通常会用两种方式：函数声明和函数表达式。

<pre>
function 函数名称(参数-可选) {}	//函数声明
</pre>

<pre>
function 函数名称-可选 (参数-可选) {}	//函数表达式
</pre>

可以看书，从外观判断，这两者最大的区别在于函数声明一定要有函数名称，而函数表达式的函数名称则可有可无。一个函数以没有函数名称的形式存在，它一定是函数表达式，那么当它有函数名称时，又该如何区分呢？JavaScript 通过上下文判断。当一个函数作为赋值表达式的一部分，它是函数表达式；若它在函数体内或程序顶端，那么它是函数声明。

来看一些 🌰：

函数声明：

<pre>
function foo(){}	//函数声明，程序的一部分

(function foo(){
	function bar(){}	//函数声明，函数体的一部分
})();
</pre>

函数表达式：

<pre>
var foo = function(){};	//函数表达式，包含在赋值表达式内
var foo = function _foo(){};	//函数表达式，包含在赋值表达式内
new function foo(){};	//函数表达式，new表达式
(function foo(){});	//函数表达式，因为包含在分组操作符内
</pre>

函数声明只能出现在程序或函数体中，从句法上讲，不能出现在块`{...}`内，如不能出现在 if、while、for 语句中，否则不同浏览器解析同一段代码时有可能出现不同结果从而导致一些奇怪的错误。

###函数声明提升

只有函数声明才能被提升，而且优先级高于变量提升，函数表达式不会如此。

<pre>
showGirl();
function showGirl(){
    alert("看！有美女！");
}
</pre>

相当于：

<pre>
function showGirl(){
    alert("看！有美女！");
}
showGirl();
</pre>

不同于变量的声明提升，赋值不提升，函数声明提升是连带整个函数体一起提升到执行环境顶端的。

<pre>
alert(foo);
var foo = "Selina";
function foo(){}
</pre>

这段代码可被解析为：

<pre>
function foo(){}
var foo;
alert(foo);
foo = "Selina";
</pre>

运行结果将弹出foo()的函数体。(非严格模式，变量重复声明被忽略)

###测试

看本文中代码的演示效果，请移步[这里](http://blog.ilanyy.com/example/hoisting/)。