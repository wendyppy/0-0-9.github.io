---
layout: post
title: "JavaScript中闭包详解"
keywords: ["JavaScript","闭包","js","Closures"]
description: "详细解读JavaScript中的闭包"
category: "JavaScript"
tags: ["JavaScript","Closures"]
---
{% include JB/setup %}

##闭包

计算机科学中，闭包（Closure）又称词法闭包（Lexical Closure）或函数闭包，是引用了自由变量的函数。这个被引用的自由变量和将函数一同存在，即使离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组成的实体。闭包运行时可有多个实例，不同的引用环境和相同的函数组合可产生不同的实例。

在一些语言中，在函数可以定义（嵌套）另一个函数时，若内部函数引用了来自外部函数的变量，则可能产生闭包。运行时，一旦外部函数被执行，一个闭包就形成了。闭包包含了内部函数的代码，以及所需外部函数变量的引用。

<i style="float:right">引用自“维基百科”</i>

来解释一下与闭包定义相关的几个概念吧。

