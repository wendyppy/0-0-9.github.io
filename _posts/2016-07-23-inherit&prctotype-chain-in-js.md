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

**1. 实例 f1、f2**<br/><br/>
f1、f2 是 Foo() 方法的实例，它们的内部属性`[[Prototype]]`指向构造函数的原型对象，也就是Foo() 方法的原型 Foo.prototype。<br /><br/>

**2. 方法 Foo()**<br/><br/>
构造函数 Foo() 的显式属性 prototype 指向了原型对象 Foo.prototype，在原型对象中定义的属性和方法可以被所有实例共享。<br/><br/>
Foo() 也是对象。它也有 `[[Prototype]]`，指向其构造函数的原型对象。既然是构造函数，那就是 Function 的实例，所以 Foo() 的`[[Prototype]]` 指向 Function.prototype。<br/><br/>
而 Function、Array 这类的内建对象的`[[Prototype]]` 都指向 Object.prototype。
<br/><br/>

**3. 原型对象 Foo.prototype**<br/><br/>
原型对象 Foo.prototype 的 constructor 属性指向构造函数 Foo()。<br/><br/>
Foo.prototype 也是对象。它也有`[[Prototype]]`，指向它的构造函数的原型对象 Object.prototype。
最后，Object.prototype 的`[[Prototype]]` 指向 null。

###基于原型链的继承

注：后续代码中，用 super 表示超类，即父类；sub 表示子类。

<pre>
function SuperFn(){
    this.desc1 = "superProp";
}
SuperFn.prototype.getSuperDesc = function(){
    return this.desc1;
};
function SubFn(){
    this.desc2 = "subProp";
}
//SubFn继承了SuperFn ①
SubFn.prototype = new SuperFn();
//添加新方法 ②
SubFn.prototype.getSubDesc = function(){
    return this.desc2;
};
//重写超类中方法 ③
SubFn.prototype.getSuperDesc = function(){
    return "Selina";
};

var child = new SubFn();
var parent = new SuperFn();

alert(child.getSuperDesc());	//"Selina"
alert(parent.getSuperDesc());	//"superProp"
</pre>

来捋顺一下代码：我们有一个超类、一个子类，超类中有方法`getSuperDesc()`。然后子类 SubFn 继承了超类 SuperFn（见①），继承是通过重写原型对象的方式实现的。此时继承了超类的子类就拥有了超类的所有属性和方法。再给子类添加新方法（见②）、重写超类中的方法（见③）。访问子类实例中的方法`getSuperDesc()`，弹出重写后的结果，超类中的同名方法被屏蔽；访问超类实例中的方法`getSuperDesc()`，保持原来不变。

哦，做个小测试，把上面代码中的先后顺序做个微调，变成 ②→③→①，结果又会如何呢？

<pre>
alert(child.getSuperDesc());	//"superProp"
alert(parent.getSuperDesc());	//"superProp"
</pre>

呵呵，在子类中重写的方法白写了，因为子类被重写了呀。所以，<span class="txt">给原型添加方法的代码一定要放在替换原型的语句之后，</span>否则就会出现上面的错误。

同样的道理，<span class="txt">在通过原型链实现继承时，不能使用对象字面量创建原型方法，</span>因为这样会重写原型链，导致原型链被切断。

<pre>
//SubFn继承了SuperFn
SubFn.prototype = new SuperFn();

//使用对象字面量添加新方法，会导致上一行代码失效
SubFn.prototype = {
	getSuperDesc: function(){
    return "Selina";
	}
};
</pre>

因为`SubFn.prototype = {...}`相当于`SubFn.prototype = new Object(...)`，SubFn.prototype 变成了 Object 的实例，而不是我们预期的 SuperFn 的实例，原型链被中断，后续操作当然会跟着出错啦。

###原型链的问题

只使用原型链来实现继承，就像只使用原型模式来创建对象一样，都会踩引用类型值和不能给（超类型的）构造函数传参的坑。当超类中有一个数组，子类继承超类后，子类的所有实例都会因为其中一个实例对数组的操作而共享这个新数组。太可怕了！还能不能有自己的私人数组了！于是实践中很少单独使用原型链。

##借用构造函数

这种技术主要是为了解决前面提到的引用类型的问题，基本思想就是在子类型构造函数的内部调用超类型构造函数。

<pre>
function SuperFn(){
    this.girls = ["Selina","Hebe","Ella"];
}
function SubFn(){
    //继承了SuperFn
    SuperFn.call(this);
}

var child1 = new SubFn();
child1.girls.push("Jay");
var child2 = new SubFn();

alert(child1.girls);	//"Selina,Hebe,Ella,Jay"
alert(child2.girls);	//"Selina,Hebe,Ella"
</pre>

函数是在特定环境中执行代码的对象，因此用`apply()`和`call()`就可以在（将来）新创建的对象上执行构造函数。

借用构造函数带来的另一个好处就是可以在子类中向超类的构造函数传递参数。像这样： ↓↓

<pre>
function SuperFn(name){
    this.name = name;
}
function SubFn(){
    SuperFn.call(this,"Selina");
    this.age = 18;
}

var child = new SubFn();
alert(child.name);	//"Selina"
alert(child.age);	//18
</pre>

###借用构造函数的问题

如果仅仅使用借用构造函数，那么就会出现构造函数模式的缺陷：方法都定义在函数中，函数复用成为空谈；子类型不可见超类型原型中定义的方法。因此，借用构造函数也很少被单独使用。

##组合继承

结合原型链和借用构造函数的技术，使用原型链实现对原型属性和方法的继承，通过借用构造函数的技术实现对实例属性的继承。

<pre>
function SuperFn(name){
    this.name = name;
    this.cds = ["super star","shero"];
}
SuperFn.prototype.sayName = function(){
    alert(this.name);
};
function SubFn(name,age){
    SuperFn.call(this,name);
    this.age = age;
}

SubFn.prototype = new SuperFn();
SubFn.prototype.sayAge = function(){
    alert(this.age);
};

SubFn.prototype.constructor = SubFn;
var child1 = new SubFn("Selina",18);
child1.cds.push("forever");
var child2 = new SubFn("Hebe",17);

alert(child1.name);	//"Selina"
alert(child1.cds);	//"super star, shero, forever"
child1.sayName();	//"Selina"
child1.sayAge();	//18

alert(child2.name);	//"Hebe"
alert(child2.cds);	//"super star, shero"
child2.sayName();	//"Hebe"
child2.sayAge();	//17
</pre>

各种结果，尽如人意。这种模式扬长避短，成为 JavaScript 中最常用的继承模式。

##测试

看本文中代码的演示效果，请移步[这里](http://blog.hardworking.top/example/inherit/)。