---
layout: post
title: "JavaScript中的变量提升"
keywords: ["JavaScript","变量提升包","js","var"]
description: "详细解读JavaScript中的变量提升"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

##执行环境

执行环境（或称“环境”）是 JavaScript 中非常重要的概念。执行环境决定了变量或函数有权访问其它数据，决定了它们各自的行为。<span class="txt">每个执行环境都有一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。</span>这个对象是我们的代码无法访问到的，供解析器后台使用。

最外层的执行环境是全局执行环境，window 对象与之关联，所有的全局变量和方法都作为 window 对象的属性和方法被创建。某个执行环境中所有代码执行完毕后，该环境被销毁，保存在其中的所有变量及函数定义也随之被销毁。全局执行环境在应用程序退出后被销毁，比如关闭网页。

每个函数都有自己的执行环境。<span class="txt">当代码在一个执行环境中执行时，会创建变量的一个**作用域链**。</span>

作用域链保证了执行环境中有权访问的变量及函数的有序访问。<span class="txt">作用域链的前端始终是当前执行代码所在环境的变量对象。</span>如果当前执行环境是函数的话，则将其**活动对象**（作用域链上正在被执行和引用的变量对象）作为变量对象。作用域链下一个变量对象来自外部环境，再下一个来自外部的外部，以此类推，直到尾端的全局执行环境的变量对象。

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


