---
layout: post
title: "JavaScript中的apply,call,bind‘/"
keywords: ["JavaScript","bind","js","apply","call"]
description: "JavaScript中apply()、call()和bind()的用法"
category: "JavaScript"
tags: ["JavaScript","Function"]
---
{% include JB/setup %}

`apply()`、`call()`、`bind()`都是 Function.prototype 上的方法，以下是它们各自的用法。

##apply

在一个对象的上下文中应用另一个对象的方法。参数以数组或类数组形式传入。

<span class="txt">`apply()`方法在指定 this 值和参数的情况下调用某个函数。</span>

在特定的作用域中调用函数，实际等于设置函数体内的 this 值。

###语法

	fun.apply(thisArg[,argsArray])

###参数

**thisArg**

fun函数运行时的 this 值。

非严格模式下，设定为 null 或 undefined 时会指向全局对象（浏览器中是 window 对象）。值为原始值（数字、字符串、布尔值）的 this 会指向该原始值的自动包装对象（Number、String、Boolean）。

**argsArray**

一个数组或类数组对象。

数组元素将作为单独的参数传递给 fun 函数；

该值为 null 或 undefined，表示不需要传递任何参数；

ES5 开始可以使用类数组对象。

###描述

apply 可以使用数组字面量，如`fun.apply(this,["Selina","Hebe"])`，或数组对象`fun.apply(this,new Array("Selina","Hebe"))`。

argsArray 参数可以使用 arguments 对象。其中，arguments 是一个类数组对象。代表传给一个function的参数列表。

ES5开始，可以使用任何形式的类数组对象，只要它有 length 属性 和 [0 ... length]整数范围。Chrome 14 以及 IE9 仍然不接受类数组对象。

###应用场景

