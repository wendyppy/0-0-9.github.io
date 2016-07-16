---
layout: post
title: "Sum All Numbers in a Range(算法)"
keywords: ["FCC","算法","freecodecamp","Sum All Numbers in a Range"]
description: "FCC算法题 Sum All Numbers in a Range"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Math"]
---
{% include JB/setup %}

###题目

我们会传递给你一个包含两个数字的数组。返回这两个数字和它们之间所有数字的合。

最小的数字并非总在最前面。

###提示

[Math.max()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

[Math.min()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/min)

[Array.reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

###思路

我记得上小学的时候有学过一个奥数公式（这公式好像初中的时候也学过吧，记不清了），求连续整数的和：

（首项 + 末项）* 项数 / 2

###解法

<pre>
function sumAll(arr) {
  return (arr[0] + arr[1])*(Math.abs(arr[0] - arr[1]) + 1)/2;
}
</pre>

###测试

sumAll([1, 4]) 应该返回一个数字。

sumAll([1, 4]) 应该返回 10。

sumAll([4, 1]) 应该返回 10。

sumAll([5, 10]) 应该返回 45。

sumAll([10, 5]) 应该返回 45。

###在线调试

[Sum All Numbers in a Range]()