---
layout: post
title: "Mutations(算法)"
keywords: ["FCC ","算法","freecodecamp","Mutations"]
description: "FCC算法题 Mutations"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

蛤蟆可以吃队友，也可以吃对手。

如果数组第一个字符串元素包含了第二个字符串元素的所有字符，函数返回true。

举例，`["hello", "Hello"]`应该返回true，因为在忽略大小写的情况下，第二个字符串的所有字符都可以在第一个字符串找到。

`["hello", "hey"]`应该返回false，因为字符串"hello"并不包含字符"y"。

`["Alien", "line"]`应该返回true，因为"line"中所有字符都可以在"Alien"找到。

###提示

[String.indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)

###思路

对第二个字符串`arr[1]`中的每个字符遍历，检查是否在第一个字符串中出现过。

###解法

<pre>
function mutation(arr) {
  var a = arr[0].toLowerCase();
  var b = arr[1].toLowerCase();
  for(var i = 0; i < b.length; i++){
    if(a.indexOf(b[i]) < 0){
      return false;
    }
  }
  return true;
}
</pre>

###测试

`mutation(["hello", "hey"])` 应该返回 false.

`mutation(["hello", "Hello"])` 应该返回 true.

`mutation(["zyxwvutsrqponmlkjihgfedcba", "qrstu"])` 应该返回 true.

`mutation(["Mary", "Army"])` 应该返回 true.

`mutation(["Mary", "Aarmy"])` 应该返回 true.

`mutation(["Alien", "line"])` 应该返回 true.

`mutation(["floor", "for"])` 应该返回 true.

`mutation(["hello", "neo"])` 应该返回 false.