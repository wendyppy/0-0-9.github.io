---
layout: post
title: "JavaScript中的递归"
keywords: ["JavaScript","递归","js"]
description: "递归在JavaScript中的应用"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

递归函数是在一个函数通过名字调用自身的情况下构成的。

<pre>
function factorial(num){
    if(num <= 1){
        return 1;
    }else{
        return num * factorial(num -1);
    }
}
</pre>

这是一个经典的递归阶乘函数。表面上看，这段代码没有任何问题，但在一些情境下，它会出错：

<pre>
var factorial2 = factorial;
factorial = null;
alert(factorial2(3));	//error
</pre>

其实很容易理解，将 factorial() 保存在 factorial2 中，再将 factorial 设为空，于是指向原始函数的引用只剩下 factorial2.而在调用 factorial2 时，在其内部又必须执行 factorial()，而 factorial 此时未定义，所以会出错。

为了解决这个问题，有人用 `arguments.callee` 代替`factorial`出现在函数体中。上面代码变成：

<pre>
function factorial(num){
    if(num <= 1){
        return 1;
    }else{
        return num * arguments.callee(num -1);
    }
}
</pre>

`callee`是`arguments`对象的属性。在函数体中，它指向现在正在执行的函数。但是，因为不能内联和尾递归、改变 this 值和效率低等问题，ES5开始已经不能在严格模式下用这个属性了。

这时候，使用命名函数表达式可以在严格模式下通用并达成相同效果。

<pre>
var factorial = (function fn(num){
    if(num <= 1){
        return 1;
    }else{
        return num * fn(num -1);
    }
});
</pre>

###测试

FreeCodeCamp 上有一道算法基础题就考察了递归算法的基本应用。[这里](http://blog.hardworking.top/fcc/fcc-algor-Factorialize-a-Number.html)是题目以及我的解法。

看本文中代码的演示效果，请移步[这里](http://blog.hardworking.top/example/recursion/)。