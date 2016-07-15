---
layout: post
title: "Finders Keepers(算法)"
keywords: ["FCC","算法","freecodecamp","Finders Keepers"]
description: "FCC算法题 Finders Keepers"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

写一个 function，它浏览数组（第一个参数）并返回数组中第一个通过某种方法（第二个参数）验证的元素。

###提示

[Array.filter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

###思路

`Array.filter()`的返回值是通过测试元素的新数组。截取到这个数组索引为0的值即可。

###解法

<pre>
function find(arr, func) {
  return arr.filter(func)[0];
}
</pre>

###测试

`find([1, 3, 5, 8, 9, 10], function(num) { return num % 2 === 0; })` 应该返回 8。

`find([1, 3, 5, 9], function(num) { return num % 2 === 0; })` 应该返回 undefined。

###在线调试

[Finders Keepers](https://freecodecamp.cn/challenges/finders-keepers)