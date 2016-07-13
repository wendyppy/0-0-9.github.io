---
layout: post
title: "Check for Palindromes(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

如果给定的字符串是回文，返回true，反之，返回false。

如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。

注意你需要去掉字符串多余的标点符号和空格，然后把字符串转化成小写来验证此字符串是否为回文。

函数参数的值可以为"racecar"，"RaceCar"和"race CAR"。

###提示

[String.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

[String.toLowerCase()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)

###思路

用正则表达式验证字符串当中的标点符号和空格，用`""`替换，最后用`toLowerCase()`方法转为小写再进行比较。

###解法

<pre>
function palindrome(str) {
  // Good luck!
  var re = /[\W\s_]/gi;
  str = str.replace(re,"");
  return str.toLowerCase() === str.split("").reverse().join("").toLowerCase();
}
</pre>

###测试

`palindrome("eye")` 应该返回一个布尔值

`palindrome("eye")` 应该返回 true.

`palindrome("race car")` 应该返回 true.

`palindrome("not a palindrome")` 应该返回 false.

`palindrome("A man, a plan, a canal. Panama")` 应该返回 true.

`palindrome("never odd or even")` 应该返回 true.

`palindrome("nope")` 应该返回 false.

`palindrome("almostomla")` 应该返回 false.

`palindrome("My age is 0, 0 si ega ym.")` 应该返回 true.

`palindrome("1 eye for of 1 eye.")` 应该返回 false.

`palindrome("0_0 (: /-\ :) 0-0")` 应该返回 true.