---
layout: post
title: "Confirm the Ending(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

检查一个字符串(str)是否以指定的字符串(target)结尾。

如果是，返回true;如果不是，返回false。

###提示

[String.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

###思路

我们来看看`substr()`的用法。

**语法：**

><pre>str.substr(start[, length])</pre>

start 是一个字符的索引。首字符的索引为 0，最后一个字符的索引为 字符串的长度减去1。substr 从 start 位置开始提取字符，提取 length 个字符（或直到字符串的末尾）。

如果 start 为正值，且大于或等于字符串的长度，则 substr 返回一个空字符串。

如果 start 为负值，则 substr 把它作为从字符串末尾开始的一个字符索引。如果 start 为负值且 abs(start) 大于字符串的长度，则 substr 使用 0 作为开始提取的索引。注意负的 start 参数不被 Microsoft JScript 所支持。

这道题目使用`substr()`的负参数运用可解决。

###解法

<pre>
function confirmEnding(str, target) {
  // "Never give up and good luck will find you."
  return str.substr(-target.length,target.length) === target;
}
</pre>

###测试

`confirmEnding("Bastian", "n")` 应该返回 true.

`confirmEnding("Connor", "n")` 应该返回 false.

`confirmEnding("Walking on water and developing software from a specification are easy if both are frozen", "specification")` 应该返回 false.

`confirmEnding("He has to give me a new name", "name")` 应该返回 true.

`confirmEnding("He has to give me a new name", "me")` 应该返回 true.

`confirmEnding("He has to give me a new name", "na")` 应该返回 false.

`confirmEnding("If you want to save our world, you must hurry. We dont know how much longer we can withstand the nothing", "mountain")` 应该返回 false.