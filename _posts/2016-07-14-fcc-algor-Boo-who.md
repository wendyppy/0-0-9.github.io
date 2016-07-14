---
layout: post
title: "Boo who(算法)"
keywords: ["FCC","算法","freecodecamp","Boo who"]
description: "FCC算法题 Boo who"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Boolean"]
---
{% include JB/setup %}

###题目

检查一个值是否是基本布尔类型，并返回 true 或 false。

基本布尔类型即 true 和 false。

###提示

[Boolean Objects](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

###思路

检测基本类型的值，可以用`typeof`操作符。

1）若变量为`undefined`类型，返回 undefined 。

2）若变量为`Boolean`类型，返回 boolean 。

3）若变量为`String`类型，返回 string 。

4）若变量为`Number`类型，返回 number 。

5）若变量为`Null`类型或引用类型，返回 object 。

6）另，由于函数其特殊性，ECMA-262规定，任何在内部实现 call 方法的对象都应在用`typeof`操作符时返回 function 。

###解法

<pre>
function boo(bool) {
  // What is the new fad diet for ghost developers? The Boolean.
  return typeof bool === "boolean";
}
</pre>

###测试

`boo(true)` 应该返回 true。

`boo(false)` 应该返回 true。

`boo([1, 2, 3])` 应该返回 false。

`boo([].slice)` 应该返回 false。

`boo({ "a": 1 })` 应该返回 false。

`boo(1)` 应该返回 false。

`boo(NaN)` 应该返回 false。

`boo("a")` 应该返回 false。

`boo("true")` 应该返回 false。

`boo("false")` 应该返回 false。

###在线调试

[Boo who](https://freecodecamp.cn/challenges/boo-who)