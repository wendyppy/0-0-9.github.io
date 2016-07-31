---
layout: post
title: "Make a Person(算法)"
keywords: ["FCC","算法","freecodecamp","Make a Person"]
description: "FCC算法题 Make a Person"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Closures"]
---
{% include JB/setup %}

###题目

用下面给定的方法构造一个对象.

方法有 getFirstName(), getLastName(), getFullName(), setFirstName(first), setLastName(last), and setFullName(firstAndLast).

所有有参数的方法只接受一个字符串参数.

所有的方法只与实体对象交互.

###提示

[Closures](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

[Details of the Object Model](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Details_of_the_Object_Model)

###思路

给定的`Person()`中有`getFirstName()`这类获取对象的方法，也有`setFirstName(first)`这类设置对象的方法，而我们在 set 类方法中定义的局部变量在函数调用结束后依然存在，能被 get 类方法访问到，这是明显的闭包。
###解法

<pre>
var Person = function(firstAndLast) {
  var arr = firstAndLast.split(" ");
  var f = arr[0];
  var l = arr[1];
  this.getFirstName = function(){
    return f;
  };
  this.getLastName = function(){
    return l;
  };
  this.getFullName = function(){
    return f + " " + l;
  };
  this.setFirstName = function(first){
     f = first;
  };
  this.setLastName = function(last){
    l = last;
  };
  this.setFullName = function(firstAndLast){
    var full = firstAndLast.split(" ");
    f = full[0];
    l = full[1];
  };
};
</pre>

###测试

`Object.keys(bob).length` 应该返回 6.

`bob instanceof Person` 应该返回 true.

`bob.firstName` 应该返回 undefined.

`bob.lastName` 应该返回 undefined.

`bob.getFirstName()` 应该返回 "Bob".

`bob.getLastName()` 应该返回 "Ross".

`bob.getFullName()` 应该返回 "Bob Ross".

`bob.getFullName()` 应该返回 "Haskell Ross" 在  `bob.setFirstName("Haskell")` 之后.

`bob.getFullName()` 应该返回 "Haskell Curry" 在 `bob.setLastName("Curry")` 之后.

`bob.getFullName()` 应该返回 "Haskell Curry" 在 `bob.setFullName("Haskell Curry")` 之后.

`bob.getFirstName()` 应该返回 "Haskell" 在 `bob.setFullName("Haskell Curry")` 之后.

`bob.getLastName()` 应该返回 "Curry" 在 `bob.setFullName("Haskell Curry")` 之后.

###在线调试

[Make a Person](https://freecodecamp.cn/challenges/make-a-person)