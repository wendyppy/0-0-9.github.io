---
layout: post
title: "Smallest Common Multiple(算法)"
keywords: ["FCC","算法","freecodecamp","Smallest Common Multiple"]
description: "FCC算法题 Smallest Common Multiple"
category: "FCC"
tags: ["FCC","高级算法","algorithm","Math"]
---
{% include JB/setup %}

###题目

找出能被两个给定参数和它们之间的连续数字整除的最小公倍数。

范围是两个数字构成的数组，两个数字不一定按数字顺序排序。

例如对 1 和 3 —— 找出能被 1 和 3 和它们之间所有数字整除的最小公倍数。

###提示

[Smallest Common Multiple](https://www.mathsisfun.com/least-common-multiple.html)

###思路

这······是一道数学题。做它之前，我们先来了解几个概念。

几个数共有的倍数叫做这几个数的公倍数，其中除0以外最小的一个公倍数，叫做这几个数的**<span class="txt">最小公倍数</span>**。

可以用公式法求得两个数的最小公倍数：

假设有自然数 a  、b，则
>a、b最小公倍数 * a、b最大公约数 = a、b的乘积

看来要求两个数的最小公倍数，我们得先从它们的最大公约数下手。

本解法中，我们用欧几里得算法（也称辗转相除法）来求两个数的**<span class="txt">最大公约数(greatest common divison)</span>**。

辗转相除法是利用以下性质来确定两个正整数 a 和 b 的最大公因子的:

<pre>
gcd(a,b) = gcd(b,a mod b)
</pre>

其中，`a mod b`表示 `a ÷ b` 的余数，它的值不为 0。

该定理可以被这样证明：

>a可以表示成a = kb + r（a，b，k，r皆为正整数），则r = a mod b

>假设d是a,b的一个公约数，记作d|a,d|b，即a和b都可以被d整除。

>而r = a - kb，两边同时除以d，r/d=a/d-kb/d=m，等式左边可知m为整数，因此d|r

>因此d也是（b,a mod b）的公约数

>因此（a,b）和（b,a mod b）的公约数是一样的，其最大公约数也必然相等，得证。

由此可得我们计算两个数最大公约数的函数表达式：

<pre>
  var gcd = function(a,b){
    if(b){
      return gcd(b, a % b);
    }
    return a;
  };
</pre>

其中`if(b)`可理解为`if(b!==0)`。

###解法

<pre>
function smallestCommons(arr) {
  var min = Math.min(arr[0],arr[1]);
  var max = Math.max(arr[0],arr[1]);
  var _arr = [];
  for(var i = min; i <= max; i++){
    _arr.push(i);
  }
  var gcd = function(a,b){
    if(b){
      return gcd(b, a % b);
    }
    return a;
  };
  return _arr.reduce(function(prev,cur,index,array){
    return prev*cur/gcd(prev,cur);
  });
}
</pre>

###测试

`smallestCommons([1, 5])` 应该返回一个数字。

`smallestCommons([1, 5])` 应该返回 60。

`smallestCommons([5, 1])` 应该返回 60。

`smallestCommons([1, 13])` 应该返回 360360。

###在线调试

[Smallest Common Multiple](https://freecodecamp.cn/challenges/smallest-common-multiple)