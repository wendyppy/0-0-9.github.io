---
layout: post
title: "Reverse a String(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Array"]
---
{% include JB/setup %}

###题目

翻转字符串

先把字符串转化成数组，再借助数组的reverse方法翻转数组顺序，最后把数组转化成字符串。

你的结果必须得是一个字符串

###提示

[Global String Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

[Array.reverse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

[Array.join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

###思路

把传入的字符串`str`转化成数组，可用`str.split('')`方法。需要注意的是，`split()`如果不传入参数，则把整个字符串作为一项，作为返回数组的内容。

借助数组的`reverse()`方法翻转数组顺序后，可以使用数组的`join()`方法生成返回的字符串。可以在`join()`中传一个空字符串`''`作为参数，这样就能把数组中的每一项直接连接。如果这里不传入参数，则`join()`默认用`,`作为分隔符连接数组各项，与题意不符。

###解法

<pre>
function reverseString(str) {
  var arr = str.split('');
  return arr.reverse().join('');
}
</pre>

###测试

`reverseString("hello")` 应该返回一个字符串

`reverseString("hello")` 应该返回 `"olleh"`.

`reverseString("Howdy")` 应该返回 `"ydwoH"`.

`reverseString("Greetings from Earth")` 应该返回 `"htraE morf sgniteerG"`.