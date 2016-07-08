---
layout: post
title: "FCC算法基础Factorialize a Number(计算一个整数的阶乘)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法","algorithm"]
---
{% include JB/setup %}

###题目
计算一个整数的阶乘

如果用字母n来代表一个整数，阶乘代表着所有小于或等于n的整数的乘积。

阶乘通常简写成 <span style="color:red">n!</span>

例如:<span style="color:red"> 5! = 1 * 2 * 3 * 4 * 5 = 120</span>
###提示
[Arithmetic Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)
###思路
输入一个数num，在函数内部进行循环操作，使num每次递减1，并与原值相乘，直至最小值递减为1.
###解法
<pre>
function factorialize(num) {
  var temp = num; 
  for(var i = num-1; i >= 1; i--){
    temp *= i;
  }
  if(num<1){return 1;}
  return temp;
}
</pre>