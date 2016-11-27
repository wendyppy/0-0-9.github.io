---
layout: post
title: "JavaScript中的执行环境与作用域链"
keywords: ["JavaScript","执行环境","js","作用域链"]
description: "详细解读JavaScript中的执行环境与作用域链"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

##执行环境

执行环境（或称“环境”）是 JavaScript 中非常重要的概念。执行环境决定了变量或函数有权访问其它数据，决定了它们各自的行为。<span class="txt">每个执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。</span>这个对象是我们的代码无法访问到的，供解析器后台使用。

与执行环境关联的变量对象包含在当前执行环境中定义的：

1. 函数的形参
2. var声明的变量
3. 函数声明（不包括函数表达式）

可以这样理解：

<pre>
执行环境 = {
	变量对象:{
		//执行环境中的数据
	}
};
</pre>

举个 🌰：

<pre>
var s = "Selina";
function showGirl(h){
	var e = "ella";
}
showGirl("Jay");
</pre>

对应的变量对象关系为：

<pre>
全局执行环境的变量对象 = {
	s: "Selina",
	showGirl: 指向函数showGirl()
};
函数showGirl()执行环境的变量对象 = {
	h: "Jay",
	e: "Ella"
}
</pre>

最外层的执行环境是全局执行环境，window 对象与之关联，所有的全局变量和方法都作为 window 对象的属性和方法被创建。某个执行环境中所有代码执行完毕后，该环境被销毁，保存在其中的所有变量及函数定义也随之被销毁。全局执行环境在应用程序退出后被销毁，比如关闭网页。

每个环境都有自己的变量对象，函数环境的变量对象是活动对象；与全局执行环境相关的全局变量对象就是全局对象自己。

<pre>
global = {
	String:
	Math:
	...
	window: global	//引用自身
}
</pre>

上面提到，当执行环境为函数时，变量对象为“活动对象”。活动对象在进入执行环境时被创建，最开始只包含 arguments 对象这一个变量。

**变量对象初始化：**

<pre>
活动对象 = {
	arguments:	//是个对象，包含length等属性
};
</pre>

接下来是全局执行环境和函数执行环境都会经历的一般行为：

**① 进入环境**

在进入执行环境时，代码执行前，变量对象已经包含下列属性：

1. 执行环境中的 this 和 arguments。（全局环境中无 arguments 对象）
2. 函数的形参 - 特指函数环境，全局环境无形参
3. 函数声明（不包括函数表达式） - <span class="txt">函数名+对应值（函数对象）</span>构成，覆盖同名变量属性
4. var声明的变量 - <span class="txt">变量名+对应值（undefined）</span>构成，不影响已声明的同名形参或函数声明

<pre>
function foo(a,b){
	alert(a);	//"Selina"
	alert(b);	//undefined
	alert(c);	//undefined
	alert(d);	//function d(){}
	alert(e);	//undefined
	alert(f);	//error
	var c = "this is c";
	function d(){}
	var e = function(){};
	(function f(){});
}
foo("Selina");
</pre>

注释内容为调用`foo()`函数后执行结果。当进入带有参数"Selina"的函数`foo()`、代码尚未执行时，活动对象表现如下：

<pre>
活动对象(foo) = {
	a: "Selina",
	b: undefined,
	c: undefined,
	d: function d(){},
	e: function d(){}
};
</pre>

`f()`不是活动对象，因为它不是一个函数声明而是一个函数表达式。

**② 执行代码**

这是处理代码的第二个阶段。这阶段开始前，活动对象已经有了属性，但它们大部分的值都还是默认的 undefined。在一阶段中，前面进入环境时的示例代码演变如下：

<pre>
活动对象(foo) = {
	a: "Selina",
	b: undefined,
	c: "this is c",
	d: function d(){},
	e: 指向定义的匿名函数表达式function(){}
};
</pre>

看懂了这些，其实就能解释一些我们以前见到的某些”诡异“的场景：

<pre>
if(true){
	var a = "Selina";
}else{
	var b = "Hebe";
}
alert(a);	//"Selina"
alert(b);	//undefined
</pre>

这里 b 的值为 undefined 而不是抛出`Uncaught ReferenceError`的异常，因为虽然 else 部分永远不会被执行，但变量 b 在进入环境阶段就已经被放在变量对象当中了。知道这一点对理解 JavaScript 的变量提升有很大帮助。

##作用域链

每个函数都有自己的执行环境。<span class="txt">当代码在一个执行环境中执行时，会创建变量的一个**作用域链**。</span>

作用域链本质上就是一个指向变量对象的指针列表，它引用但不包含变量对象。这个列表中有内部环境所有变量对象（包括可访问到的父变量对象）。

函数执行环境的作用域链在函数调用时创建，包含这个函数的活动对象和函数的`[[scope]]`属性。

`[[scope]]`是所有父级变量对象的层级链。在函数创建时即作为函数的静态属性存在，不可更改，一直存在直至函数被销毁（即使函数不被调用，它的`[[scope]]`属性也是存在的）。PS.通过构造函数创建的函数是例外，它们的`[[scope]]`属性总是唯一的全局对象。

下面是函数被调用时作用域链的构成：

<pre>作用域链 = 执行环境活动对象|变量对象 + [[scope]];</pre>

可以看到，当前环境的活动对象总是位于作用域链的前端。也就是说，当标识符的查找开始，先从自身属性开始查找，然后再搜寻父环境。

<pre>
var girl = "Selina";
function showGirl(){
    var girl2 = "Hebe";
    function swapGirl(){
        var temp = girl;
        girl = girl2;
        girl2 = temp;
        //可访问 temp,girl,girl2
    }
    //可访问 girl,girl2
    swapGirl();
}
//只能访问 girl
showGirl();

alert(girl);	//"Hebe"
alert(girl2);	//error
</pre>

这段代码的作用域链关系如下图：

![](http://cdn.saymagic.cn/o_1ap1bv7vgmni1opo1rhc1cs7178t9.png)

图中不同色块分别代表不同的执行环境，内部环境可以通过作用域链访问所有外部环境，但外部不能访问内部的变量和函数。例如，`swapGirl()`作用域链包含`swapGirl()`变量对象、`showGirl()`变量对象、全局变量对象这3个对象；`showGirl()`的作用域链包含它自己的变量对象和全局变量对象这2个对象，它不能访问内部函数`swapGirl()`的执行环境。

对函数`showGirl()`来说，它的作用域链：

<pre>
scope = showGirl.活动对象 + [[scope]]
</pre>

`showGirl()`的 [[scope]]：

<pre>
showGirl.[[scope]] = {
	全局上下文.变量对象
};
</pre>

`swapGirl()`被调用时，当前活动变量被添加到作用域链前端：

<pre>
scope = [swapGirl.活动对象].concat([[scope]])
</pre>

作用域链保证了执行环境中有权访问的变量及函数的有序访问。<span class="txt">作用域链的前端始终是当前执行代码所在环境的变量对象。</span>如果当前执行环境是函数的话，则将其**活动对象**（作用域链上正在被执行和引用的变量对象）作为变量对象。作用域链下一个变量对象来自外部环境，再下一个来自外部的外部，以此类推，直到尾端的全局执行环境的变量对象。

###测试

看本文中代码的演示效果，请移步[这里](http://blog.hardworking.top/example/hoisting/)。