---
layout: post
title: "JavaScript中的对象创建模式"
keywords: ["JavaScript","对象","js","构造函数模式","原型模式"]
description: "深入理解JavaScript中的对象、常用的创建对象模式"
category: "JavaScript"
tags: ["JavaScript","Object"]
---
{% include JB/setup %}

在 JavaScript 中，“一切皆对象”（除了原生类型的 null 和 undefined）。

对象的每个属性或者方法都有一个名字，每个名字都映射到一个值。我们可以把对象想象成名值对组成的散列表，其中值可以是数据或者函数。

每个对象都是基于一个引用类型创建的，这个引用类型可以是原生类型，也可以是开发者自定义的类型。

创建新对象有以下两种方式：

**1.直接创建 Object 的实例，然后为其添加属性和方法。**

<pre>
var girl = new Object();
girl.name = "Selina";
girl.age = 18;
girl.sayHello = function(){
  alert(this.name + " says Hello!");
};
</pre>

这段代码也可以用对象字面量语法重写：

<pre>
var girl = {
	name: "Selina",
	age: 18,
	
	sayHello: function(){
		alert(this.name + " says Hello!");
	}
};
</pre>

**2.用函数定义对象，再创造对象的实例。**

JavaScript 虽然支持面向对象编程，但并不使用类或者接口。ES6中新增了 class 关键字，但这里的 class 不是新的对象继承模型，而是原型链的语法糖，JavaScript 依旧是基于原型的。

在没有类的情况下，可以使用以下模式创建对象。

##工厂模式

思路：使用简单的函数创建对象，为对象添加属性、方法，然后返回对象。

示例代码：

<pre>
function createGirl(name,age){
  var obj = new Object();
  obj.name = name;
  obj.age = age;
  obj.sayHello = function(){
    alert(this.name + " says Hello!");
  };
  return obj;
}
var girl1 = createGirl("Selina",18);
var girl2 = createGirl("Hebe",17);
</pre>

优点：解决了创建多个相似对象的问题。

缺点：无法知道一个对象的类型。后被构造函数模式取代。

##构造函数模式

示例代码：

<pre>
function Girl(name,age){
  this.name = name;
  this.age = age;
  this.sayHello = function(){
    alert(this.name + " says Hello!");
  };
}
var girl1 = new Girl("Selina",18);
var girl2 = new Girl("Hebe",17);
</pre>

本例中，`Person()` 替代了`createPerson()`。借鉴了其它面向对象语言，构造函数是以一个大写字母开头的。在`Person()`内部，没有显式地创建对象，而是直接将属性和方法赋给了 this 对象。不必明确使用 return 语句，默认情况下构造函数返回 this（指向的新对象）。

要创建 Person 的新实例，必须使用 new 操作符。这也是构造函数和普通函数的区别之一。用 new 调用的函数都能作为构造函数，不用 new 调用的都是普通函数。

那么，用 new 实例化的时候的时候都会经历哪些步骤呢？以`var girl1 = new Girl("Selina",18);`这句代码为例，我们来分析一下。

1. 创建一个新对象：<code>var girl1 = {};</code>

2. 将构造函数的作用域赋给新对象。也就是说，`Person()`中的`this`将指向新对象`girl1`。

3. 执行构造函数中的代码。为`girl1`添加`name`、`age`属性和`sayHello()`方法。

4. 返回新对象`girl1`。

优点：前面提到过，工厂模式无法标识对象的类型，而创建自定义的构造函数则能把它的实例标识为一种特定类型。

对象的 constructor 属性可以用来标识对象类型。

<pre>
alert(girl1.constructor == Girl);//true
</pre>

当然啦，你也可以用 instanceof 操作符进行类型检测。

<pre>
alert(girl1 instanceof Girl);//true
alert(girl1 instanceof Object);//true
</pre>

缺点：每个方法都要在每个实例上重新创建一遍。

`girl1`和`girl2`虽然都有叫`sayHello()`的方法，但这两个方法并不是同一个Function的实例。JavaScript 中的函数是对象，所以每定义一个函数，其实也是实例化了一个对象。

前例中的`this.sayHello = function(){
    alert(this.name + " says Hello!");
  };`也能理解为`this.sayHello = new Function(alert(this.name + " says Hello!"););`。
  
不想创建多个完成同样任务的Function实例，可以在构造函数外部来定义函数：

<pre>
function Girl(name,age){
  this.name = name;
  this.age = age;
  this.sayHello = sayHello;
}
function sayHello(){
	alert(this.name + " says Hello!");
}

var girl1 = new Girl("Selina",18);
var girl2 = new Girl("Hebe",17);
</pre>

把`sayHello()`放在构造函数外部定义，而构造函数内部只需将 sayHello 属性指向全局函数`sayHello()`的指针，`girl1`和`girl2`即可共享全局作用域中的同一个`sayHello()`函数，解决了每个方法都要在每个实例上重新创建的问题。但是这样做又会带来新的麻烦：某个对象才能用到的方法都要定义到全局作用域，如果对象要用到很多方法呢？代码的逻辑性、自定义函数的封装性都会受到很大影响。

为解决这样的问题，原型模式应运而生。

##原型模式

这个模式利用了每个函数都有的 prototype 属性。

示例代码：

<pre>
function Girl(){}

Girl.prototype.name = "Selina";
Girl.prototype.age = 18;
Girl.prototype.sayHello = function(){
    alert(this.name + " says Hello!");
};

var girl1 = new Girl();
girl1.sayHello();//"Selina says Hello!"

var girl2 = new Girl();
girl2.sayHello();//"Selina says Hello!"

alert(girl1.sayHello == girl2.sayHello);//true
</pre>

可以看出，在`Girl.prototype`上创建的属性和方法，是被所有实例共享的。

任何时候，只要创建了一个函数，该函数就会拥有 prototype 属性，该属性指向函数的原型对象。默认情况下，所有原型对象都自动获得一个 constructor 属性，该属性包含一个指向 prototype 属性所在函数的指针。恩，这句话有点绕，我们来看图说话：

![图3-1](http://cdn.saymagic.cn/o_1ao96r6uocvj2hm1tdu1uso1p6p9.png)

上图展示了示例代码中各个对象之间的关系。我们来稍作梳理：

<span class="txt">创建了自定义构造函数后，其原型对象默认只会取得 constructor 属性；其他方法皆从 Object 继承而来。</span>创建构造函数 Girl 后，Girl 的原型对象自动获得 constructor 属性。

<span class="txt">当调用构造函数创建一个新实例后，该实例内部将包含一个指针，指向**构造函数的原型对象**。</span>Girl 的每个实例 girl1 、girl2 都包含了指向Girl的原型属性`Girl.prototype`的指针，而与构造函数没有直接关系。

当代码要读取某个对象的某个属性时，都会执行一次搜索。先查询对象实例是否有同名属性，没有的话，再访问原型对象。举个🌰（←←栗子），当我们调用示例代码中的`girl1.sayHello()`时，解析器会问：“实例 girl1 有 sayHello 属性吗？”答曰：“无。”继续搜索，解析器又问：“girl1 的原型有 sayHello 属性吗？”答曰：“有。”于是读取保存在原型对象中的 sayHello 函数。 Girl 的其他实例想要访问 sayHello 属性时，重现的也是相同的过程。这即是多个对象实例共享原型所保存的属性和方法的基本原理。

那么，当对象实例和原型对象中出现同名属性时会返回哪个呢？当然是对象实例上的属性啦，因为解析器搜索的时候先访问它，找到结果后也就不必进行二次搜素了，原型对象上的同名属性将被屏蔽。这个时候如果用 delete 删除了实例属性，原型对象上的同名属性就又可以访问啦。像酱紫：

<pre>
function Girl(){}

Girl.prototype.name = "Selina";
Girl.prototype.age = 18;
Girl.prototype.sayHello = function(){
    alert(this.name + " says Hello!");
};

var girl1 = new Girl();
var girl2 = new Girl();

girl1.name = "Hebe";
alert(girl1.name);	//"Hebe"，来自实例
alert(girl2.name);	//"Selina"，来自原型

delete girl1.name;
alert(girl1.name);	//"Selina"，来自原型
</pre>

**`hasOwnProperty()`方法可以检测属性存在与原型还是实例。该方法只有在给定属性存在于对象实例中时，才返回 true。**

<pre>
alert(girl2.hasOwnProperty("name"));  //false
</pre>

**单独使用 in 操作符可以判断通过对象是否能访问给定属性。换句话说，不管是原型还是实例，只要存在要访问的属性，就返回 true。**

<pre>
alert("name" in girl2);	//true
</pre>

结合`hasOwnProperty()`方法和 in 操作符，就能确定一个属性到底是存在于对象实例当中还是原型当中。下面这个函数在确定属性是原型中的属性时返回 true。

<pre>
function PrototypeProperty(obj,name){
	return !obj.hasOwnProperty(name) && (name in obj);
}
</pre>

原型模式的代码可以用对象字面量的语法重写：

<pre>
function Girl(){}

Girl.prototype = {
	name: "Selina",
	age: 18,
	sayHello: function(){
    alert(this.name + " says Hello!");
	}
};
</pre>

要注意的是，这样重写后，虽然结果相同，但 constructor 属性指向 Object，而不再是 Girl 了。之前提到过，每创建一个函数，就会自动创建它的 prototype 对象，这个 prototype 对象也会自动获得 constructor 属性。而这里的语法，本质上重写了默认的 prototype 对象，因而 constructor 属性也随之变为新对象的 constructor 属性。此时通过 constructor 已经无法确定对象的类型了。

<pre>
var girl = new Girl();
alert(girl instanceof Girl);		//true
alert(girl.constructor == Girl);	//false
alert(girl.constructor == Object);	//true
</pre>

如果你需要用到 constructor 值，可以这样显示地声明它：

<pre>
function Girl(){}

Girl.prototype = {
	constructor: Girl,
	name: "Selina",
	age: 18,
	sayHello: function(){
    alert(this.name + " says Hello!");
	}
};
</pre>

还有很重要的一点，要先为原型添加属性和方法，再调用构造函数创建实例。这两者顺序一定不能变。来一个**错误**示范：

<pre>
function Girl(){}

var girl = new Girl();

Girl.prototype = {
	constructor: Girl,
	name: "Selina",
	age: 18,
	sayHello: function(){
    alert(this.name + " says Hello!");
	}
};

girl.sayHello();	//error
</pre>

我们知道，调用构造函数时会为实例添加一个指向原型对象的[[Prototype]]指针。调用构造函数之后再重写整个原型对象相当于把原型修改为一个新对象。而此时 Girl 中的指针指向的还是除了自动获得的 constructor 属性外一无所有的最初的原型。用 Girl 的实例去调用一个它根本没有的方法，当然会出错啦。<span class="txt">实例中的指针仅指向原型，而不指向构造函数。</span>

缺点：

1. 这里的构造函数没有参数。也就是说，默认情况下所有实例取得相同的属性值。
2. 原型中所有属性被实例共享，这种共享适合于函数，包含基本值的属性也说得过去（因为你可以在实例上重写同名属性），但用在包含引用类型的属性上，就会出现问题。

<pre>
function Girl(){}

Girl.prototype = {
	constructor: Girl,
	name: "Selina",
	age: 18,
	friends: ["Hebe","Ella"],
	sayHello: function(){
    alert(this.name + " says Hello!");
	}
};

var girl1 = new Girl();
var girl2 = new Girl();

girl1.friends.push("Jay");

alert(girl1.friends);					//"Hebe,Ella,Jay"
alert(girl2.friends);					//"Hebe,Ella,Jay"
alert(girl1.friends === girl2.friends);	//true
</pre>

可以看到，girl1 和 girl2 想要有不同的 friends 属性根本是做不到的。不过没关系，团结力量大，联合使用构造函数模式和原型模式，你将走上人生巅峰。

##组合使用构造函数模式和原型模式

构造函数模式定义实例属性，原型模式定义方法和共享的属性。

这样一来，每个实例都会有自己的实例属性的 copy，同时又能共享方法的引用。另外，你也可以向构造函数传递参数了。

示例代码：

<pre>
function Girl(name,age){
	this.name = name;
	this.age = age;
	this.friends = ["Hebe","Ella"];
}

Girl.prototype = {
	constructor: Girl,
	sayHello: function(){
    alert(this.name + " says Hello!");
	}
};

var girl1 = new Girl("Selina",18);
var girl2 = new Girl("Angela",22);

girl1.friends.push("Jay");
alert(girl1.friends);					//"Hebe,Ella,Jay"
alert(girl2.friends);					//"Hebe,Ella"
alert(girl1.friends === girl2.friends);	//false
alert(girl1.sayHello === girl2.sayHello);//true
</pre>

构造函数中定义的实例属性可以私有，原型中定义的方法共享。完美！