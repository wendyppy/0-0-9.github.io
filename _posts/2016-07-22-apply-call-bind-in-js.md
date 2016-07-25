---
layout: post
title: "JavaScript中的apply,call,bind"
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

####语法

	fun.apply(thisArg[,argsArray])

####参数

**thisArg**

fun函数运行时的 this 值。

非严格模式下，设定为 null 或 undefined 时会指向全局对象（浏览器中是 window 对象）。值为原始值（数字、字符串、布尔值）的 this 会指向该原始值的自动包装对象（Number、String、Boolean）。

**argsArray**

一个数组或类数组对象。

数组元素将作为单独的参数传递给 fun 函数；

该值为 null 或 undefined，表示不需要传递任何参数；

ES5 开始可以使用类数组对象。

####描述

apply 可以使用数组字面量，如`fun.apply(this,["Selina","Hebe"])`，或数组对象`fun.apply(this,new Array("Selina","Hebe"))`。

argsArray 参数可以使用 arguments 对象。其中，arguments 是一个类数组对象。代表传给一个function的参数列表。

ES5开始，可以使用任何形式的类数组对象，只要它有 length 属性 和 [0 ... length]整数范围。Chrome 14 以及 IE9 仍然不接受类数组对象。

####应用场景

**两数求和**

<pre>
function sum(num1,num2){
	return num1 + num2;
}
function callSum1(num1,num2){
	//传入 arguments 对象
	return sum.apply(this,arguments);
}
function callSum2(num1,num2){
	//传入数组
	return sum.apply(this,[num1,num2]);
}
alert(callSum1(5,6)); //11
alert(callSum2(5,6)); //11
</pre>

可见，`apply()`方法的第二个参数用 arguments 或参数数组都能返回相同值。

**找到数组中的最大/最小值**

<pre>
var arr = [1,2,3,4,5];
var max = Math.max.apply(Math,arr);
var min = Math.min.apply(max,arr);
alert(max);	//5
alert(min);	//1
</pre>

测试了一下，这里`apply()`方法的第一个参数，可填`Math`、`window`、`this`、`null`、`undefined`中的任意一种，甚至 Math 的内置的 `max`、`min`也都可以正确运行。在函数内部加了`"use strict";`设置了严格模式也是如此。

还有，这样使用 `apply()` 的时候还要注意参数个数越界的问题，当然了，我指的是你传入的参数非常多的情况下。因为有些引擎会限制传入到方法的参数个数。

**两数组合并**

<pre>
var arr1 = [1,2,3];
var arr2 = ["Selina","Hebe","Ella"];
Array.prototype.push.apply(arr1,arr2);
alert(arr1);	//"1,2,3,Selina,Hebe,Ella"
alert(arr2);	//"Selina,Hebe,Ella"
</pre>

这里相当于调用 arr1 原型中的 push() 方法推入 arr2 值。