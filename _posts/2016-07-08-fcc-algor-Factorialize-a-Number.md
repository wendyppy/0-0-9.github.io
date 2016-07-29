---
layout: post
title: "Factorialize a Number(算法)"
keywords: ["FCC ","算法","freecodecamp","Factorialize a Number"]
description: "FCC算法题 Factorialize a Number"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Operators"]
---
{% include JB/setup %}

###题目
计算一个整数的阶乘

如果用字母n来代表一个整数，阶乘代表着所有小于或等于n的整数的乘积。

阶乘通常简写成 <span class="txt">n!</span>

例如:<span class="txt"> 5! = 1 * 2 * 3 * 4 * 5 = 120</span>
###提示
[Arithmetic Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)
###思路

**解法一**

输入一个数num，在函数内部进行循环操作，使num每次递减1，并与原值相乘，直至最小值递减为1.

**解法二**

使用经典递归。

**解法三**

使用命名函数表达式。

###解法

**解法一**

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

**解法二**

<pre>
function factorialize(num) {
 if(num <= 1){
   return 1;
 }else{
   return num * factorialize(num - 1);
 }
}
</pre>

**解法三**

<pre>
var factorialize = (function fn(num){
	if(num <= 1){
   		return 1;
	}else{
  		return num * fn(num - 1);
 	}
});
</pre>

###测试
<span class="txt">factorialize(5)</span> 应该返回一个数字

<span class="txt">factorialize(5)</span> 应该返回 120.

<span class="txt">factorialize(10)</span> 应该返回 3628800.

<span class="txt">factorialize(20)</span> 应该返回 2432902008176640000.

<span class="txt">factorialize(0)</span> 应该返回 1.

###在线调试

[Factorialize a Number](https://freecodecamp.cn/challenges/factorialize-a-number)