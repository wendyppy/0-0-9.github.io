---
layout: post
title: "Sum All Odd Fibonacci Numbers(算法)"
keywords: ["FCC","算法","freecodecamp","Sum All Odd Fibonacci Numbers"]
description: "FCC算法题 Sum All Odd Fibonacci Numbers"
category: "FCC"
tags: ["FCC","高级算法","algorithm","Operators"]
---
{% include JB/setup %}

###题目

返回所有小于传入数值的斐波那契数列中的奇数之和，如果传入的数值是斐波那契数，那么它也应该参与求和。

斐波那契数列中的前几个数字是 1、1、2、3、5 和 8，随后的每一个数字都是前两个数字之和。

例如，向 function 传入 4 应该返回 5，因为斐波那契数列中所有小于 4 的奇数是 1、1、3。

###提示

[Remainder](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Remainder_(.25))

###思路

首先，我们需要一个斐波那契数列衍生的子数组。它的初始值就是斐波那契数列的前两项：

<pre>
var fibo = [1, 1];
</pre>

我们还需要一个变量来记载当前奇数项的和：

<pre>
var oddSum = 2;
</pre>

用`while(true)`创建一个循环，在循环中求函数的返回值。

<pre>
while(true){
	//do something...
}
</pre>

设置变量`item`，它的值是下一个斐波那契数的值。

<pre>
var item = fibo[0] + fibo[1];
</pre>

看题目要求，“返回所有小于传入数值的斐波那契数列中的奇数之和，如果传入的数值是斐波那契数，那么它也应该参与求和”。如果传入的`num`大于或等于下一个将要经历的斐波那契数的话，循环将继续执行。也就是说，如果`num`小于下一个斐波那契数`item`，循环将停止，整个函数返回`oddSum`。

<pre>
if(num < item){
      return oddSum;
    }
</pre>

当下一个斐波那契数`item`是奇数的话，把它加到`oddSum`中：

<pre>
if(item % 2){
      oddSum += item;    
    }
</pre>

其中，`item % 2`得出结果是`Number`类型的，如果值为 +0, -0 或 NaN 时，在转为`Boolean`类型时会转为false。也就是说，这个条件语句在`item`为奇数时执行。

最后，更新数组。

<pre>
fibo[0] = fibo[1];
fibo[1] = item;
</pre>

###解法

<pre>
function sumFibs(num) {
  var fibo = [1, 1];
  var oddSum = 2;
  while(true){
    var item = fibo[0] + fibo[1];
    if(num < item){
      return oddSum;
    }
    if(item % 2){
      oddSum += item;    
    }
    fibo[0] = fibo[1];
    fibo[1] = item;
  }
}
</pre>

###测试

`sumFibs(1)` 应该返回一个数字。

`sumFibs(1000)` 应该返回 1785。

`sumFibs(4000000)` 应该返回 4613732。

`sumFibs(4)` 应该返回 5。

`sumFibs(75024)` 应该返回 60696。

`sumFibs(75025)` 应该返回 135721。

###在线调试

[Sum All Odd Fibonacci Numbers](https://freecodecamp.cn/challenges/sum-all-odd-fibonacci-numbers)