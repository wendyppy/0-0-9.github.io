---
layout: post
title: "JavaScript中的继承与原型链"
keywords: ["JavaScript","继承","js","原型"]
description: "JavaScript中的继承与原型链"
category: "JavaScript"
tags: ["JavaScript"]
---
{% include JB/setup %}

许多OO语言都有接口继承和实现继承两种继承方式，它们分别继承方法签名和实际的方法。而ECMAScript因为函数没有签名故而只支持实现继承。涉及到继承这一块，JavaScript 只有一种结构，即对象。JavaScript 的实现继承是依靠原型链来实现的。

##原型链

当访问一个对象的属性时，在该对象上搜寻 → 搜寻该对象的原型 → 搜寻该对象原型的原型...，依次层层向上搜索，直到找到一个名字匹配的属性或者到达原型链末尾（null，即不再有原型指向）。这样子的属性搜寻过程是不是很像顺着一条链子找东西呢？

分享一个神图：

![](http://cdn.saymagic.cn/o_1aom2dgfs16b73c3tpa1ie06s89.jpg)

