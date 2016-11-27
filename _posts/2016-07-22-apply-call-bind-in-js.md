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

`apply()`方法在<span class="txt">指定 this 值和参数的情况下调用某个函数。</span>

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

这里`apply()`方法的第一个参数，可填`Math`、`window`、`this`、`null`、`undefined`中的任意一种，甚至之前声明的 `max`、`min`，或者`new Object()`等等，只要输入合法，都可以正确运行。因为 apply 的这个操作用不到 this 值，所以随你玩。在函数内部加了`"use strict";`设置了严格模式也是如此。

还有，这样使用 `apply()` 的时候还要注意参数个数越界的问题，当然了，我指的是你传入的参数非常多的情况下。因为有些引擎会限制传入到方法的参数个数。

**两数组合并**

<pre>
var arr1 = [1,2,3];
var arr2 = ["Selina","Hebe","Ella"];
Array.prototype.push.apply(arr1,arr2);
alert(arr1);	//"1,2,3,Selina,Hebe,Ella"
alert(arr2);	//"Selina,Hebe,Ella"
</pre>

这里相当于调用 arr1 的 push() 方法推入 arr2 值。

##call

`call()`方法与`apply()`方法作用相同，区别仅在于接收参数的方式。第一个参数 this 值和 `apply()`相同，变化的是剩余的所有参数都直接传给函数。也就是说，使用`call()`方法时，传给函数的参数必须逐个列举出来。

###语法

	fun.call(thisArg[,arg1[,arg2[,...]]])

###参数

**thisArg**

fun函数运行时的 this 值。

非严格模式下，设定为 null 或 undefined 时会指向全局对象（浏览器中是 window 对象）。值为原始值（数字、字符串、布尔值）的 this 会指向该原始值的自动包装对象（Number、String、Boolean）。

**arg1,arg2,...**

指定的参数列表。

###描述

通过`call()`，可以<span class="txt">在一个对象上借用另一个对象的方法。</span>

如：`Object.prototype.toString.call([])`，是一个 Array 对象借用了 Object 对象上的方法。

###应用场景

**两数求和**

<pre>
function sum(num1,num2){
  return num1 + num2;
}
function callSum(num1,num2){
  return sum.call(this,num1,num2);
}
alert(callSum(3,4));	//7
</pre>

**扩充函数运行的作用域**

<pre>
window.girl = "Selina";
var o = {girl: "Hebe"};

function showGirl(){
    alert(this.girl);
}

showGirl();				//"Selina"
showGirl.call(this);	//"Selina"
showGirl.call(window);	//"Selina"
showGirl.call(o);		//"Hebe"
</pre>

本例中，`showGirl()`是作为全局函数定义的，在全局作用域中调用时，函数体中的`this.girl`会转化成`window.girl`，其值为`Selina`。
`showGirl.call(this)`，`showGirl.call(window)`是两种显式地在全局作用域中调用函数的方式。
而运行`showGirl.call(o)`时，函数体内 this 指向 o ，于是结果为`Hebe`。

用`call()`或`apply()`这样扩充作用域，对象不需要与方法有任何耦合关系。

**示例3**

<pre>
var girl = {
    name: "Selina",
    sayHello: function(obj){
        alert(this.name + " says Hello " + obj)
    }
};
girl.sayHello.call(girl,"Everybody");
</pre>

可以看出，`call()`将 this 值绑定给函数后函数是立即执行的。写这个是为了和后面的`bind()`对比。

##bind

这个方法会创建一个函数的实例，其 this 值会被绑定到传给`bind()`函数的值。

###语法

	fun.bind(thisArg[,arg1[,arg2[,...]]])

###参数

`bind()`会创建一个新函数（称为绑定函数）。当这个新函数被调用时，它的 this 值是传递给`bind()`的第一个参数，它的参数是`bind()`的其他参数和其原本的参数。

**thisArg**

当绑定函数被调用时，该参数会作为原函数的 this 指向；

当使用 new 调用绑定函数时，该参数无效。

**arg1,arg2,...**

绑定函数被调用时，这些参数加上绑定函数原本的参数会按照顺序作为原函数运行时的参数。

###描述

新函数（绑定函数）与被调函数（目标函数）具有相同的函数体。目标函数被调用时 this 值绑定到`bind()`的第一个参数，该参数不能被重写。绑定函数被调用时，`bind()`也接受预设的参数提供给原函数。一个绑定函数也能使用 new 创建对象：这种行为就像把原函数当成构造器，忽略提供的 this 值，同时模拟函数接收调用时的参数。

###应用场景

**创建绑定函数**

<pre>
this.girl = "Selina";
var star = {
  girl: "Hebe",
  myGirl: function(){
    return this.girl;
  }
};
var thisGirl = star.myGirl;
var boundThisGirl = thisGirl.bind(star);

alert(star.myGirl());		//"Hebe"
alert(thisGirl());			//"Selina"
alert(boundThisGirl());		//"Hebe"
</pre>

将 star 的 `myGirl()` 方法在函数外调用，因为 this 的指向变了，所以有时会出现问题（对象丢失）。 如 `thisGirl()`调用时就结果就变成了全局作用域下的`Selina`。这时候创建一个绑定函数则能避免此类问题的发生。

**为函数预设初始参数**

<pre>
function list(){
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1,2,3);

var presetList = list.bind(undefined,"Selina");
var list2 = presetList();
var list3 = presetList(4,5,6);

alert(list1);		//"1,2,3"
alert(list2);		//"Selina"
alert(list3);		//"Selina,4,5,6"
</pre>

`Array.prototype.slice.call(arguments)`相当于 arguments 借用了数组的`slice()`方法，所以也可以写成`[].slice.call(arguments)`，或者直接用`bind()`方法将`list()`函数重写成这样：

<pre>
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);
function list(){
	return slice(arguments);
}
</pre>

可以看出，本例中的`list()`方法是把一个类数组对象转换为数组，list1 的结果其实是`[1,2,3]`，因为在弹出框中显示所以没有数组边界符。后面的 list2、list3 同理。

**示例3**

写这个是为了与 call 中的示例3对比。来看看`call()`中的代码用`bind()`写会是什么样子。

<pre>
var girl = {
    name: "Selina",
    sayHello: function(obj){
        alert(this.name + " says Hello " + obj)
    }
};
//girl.sayHello.call(girl,"Everybody");
var boundHello = girl.sayHello.bind(girl);
boundHello("Everybody");
</pre>

被注释掉的内容是用`call()`实现相同效果应用的代码。

或者最后两行可以写成这样：

<pre>
var boundHello = girl.sayHello.bind(girl,"Everybody");
boundHello();
</pre>

这里就能看出`call()`和`bind()`的区别来了：`call()`绑定 this 后立即自动调用，`bind()`绑定 this 后需要单独调用。

##测试

看本文中代码的演示效果，请移步[这里](http://blog.hardworking.top/example/apply-call-bind/)。