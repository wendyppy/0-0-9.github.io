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

<center>![](http://cdn.saymagic.cn/o_1aom2dgfs16b73c3tpa1ie06s89.jpg)</center>

###隐式原型

在图中，我们看到了`_proto_`这个东西，而在一些类似的场景中，我们又看到`[[Prototype]]`代替`_proto_`出现。它们分别都是什么呢？为了更好地理解原型链的概念，我们先来说说这二者都是什么。

为了实现 JavaScript 引擎，ECMA-262 规定了一些只有内部才用的特性（在 JavaScript 中不能直接访问），并把它们放在了两对中括号中。

JavaScript 中的任意对象都有一个指针（内部属性），指向创建它的函数的 prototype*(to the value of its constructor’s "prototype")*。ECMA-262 第5版中把这个指针叫做`[[Prototype]]`。在脚本中没有标准的方式能够访问`[[Prototype]]`，但 Chrome、Firefox、Safari、IE9 等浏览器在每个对象上都支持一个属性`_proto_`；而在其它实现中，这个属性对脚本是完全不可见的。可以说，`_proto_`是为了访问`[[Prototype]]`的浏览器实现。ES5增加了`Object.getPrototypeOf()`方法，能够返回`[[Prototype]]`值。

这里的`[[Prototype]]`(也可理解为`_proto_`）就是**隐式原型**。

###对象 & 方法

<span class="txt">每个对象都有隐式原型。一个对象的`[[Prototype]]`指向构造它的构造函数的 prototype，</span>这就是为什么实例能访问其构造函数原型中的属性和方法。

<span class="txt">方法（Function）也是个特殊的对象，除了隐式原型`[[Prototype]]`外，还有自己特有的显式属性 prototype。该属性指向包含所有实例共享的属性和方法的原型对象。</span>原型对象有一个 constructor 属性指向原构造函数。

###关于例图

观察上面的例图，可以发现图中实体大致可以从左至右分成三类：实例、方法、方法的原型对象。我们现在来一一分析。

1. 实例 f1、f2

	f1、f2 是 Foo() 方法的实例，它们的内部属性`[[Prototype]]`指向构造函数的原型对象，也就是Foo() 方法的原型 Foo.prototype。<br />

2. 方法 Foo()

	构造函数 Foo() 的显式属性 prototype 指向了原型对象 Foo.prototype，在原型对象中定义的属性和方法可以被所有实例共享。
	
	 Foo() 也是对象。它也有 `[[Prototype]]`，指向其构造函数的原型对象。Foo() 的构造函数是 Function，所以它的 `[[Prototype]]` 指向 Function.prototype。

3. 原型对象 Foo.prototype

原型对象 Foo.prototype 的 constructor 属性指向构造函数 Foo()。