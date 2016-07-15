---
layout: post
title: "Binary Agents(算法)"
keywords: ["FCC","算法","freecodecamp","Binary Agents"]
description: "FCC算法题 Binary Agents"
category: "FCC"
tags: ["FCC","中级算法","algorithm","String"]
---
{% include JB/setup %}

###题目

传入二进制字符串，翻译成英语句子并返回。

二进制字符串是以空格分隔的。

###提示

[String.charCodeAt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

[String.fromCharCode()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

###思路

把传进来的字符串用空格切割为数组。遍历数组每一项，把二进制转为十进制之后用`String.fromCharCode()`方法得到对应的字符。

最后把数组重新连接成字符串，搞定。

###解法

<pre>
function binaryAgent(str) {
  var arr = str.split(" ");
  arr = arr.map(function(item,index,array){
    return String.fromCharCode(parseInt(item,2));
  });
  
  return arr.join("");
}
</pre>

###测试

`binaryAgent("01000001 01110010 01100101 01101110 00100111 01110100 00100000 01100010 01101111 01101110 01100110 01101001 01110010 01100101 01110011 00100000 01100110 01110101 01101110 00100001 00111111")` 应该返回 "Aren't bonfires fun!?"

`binaryAgent("01001001 00100000 01101100 01101111 01110110 01100101 00100000 01000110 01110010 01100101 01100101 01000011 01101111 01100100 01100101 01000011 01100001 01101101 01110000 00100001")` 应该返回 "I love FreeCodeCamp!"

###在线调试

[Binary Agents](https://freecodecamp.cn/challenges/binary-agents)