---
layout: post
title: "Repeat a string repeat a string(FCC)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm"]
---
{% include JB/setup %}

###题目
重要的事情说3遍！

重复一个指定的字符串<span class="txt">num</span>次，如果<span class="txt">num</span>是一个负数则返回一个空字符串。
###提示
[Global String Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
###思路
使用[String.prototype.repeat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)方法可简单实现。

`repeat()` 构造并返回一个重复当前字符串若干次数的新字符串.
###解法
<pre>
function repeat(str, num) {
  // repeat after me
  if(num < 0){
    return "";
  }else{
   return str.repeat(num); 
  }
}
</pre>
###测试
`repeat("*", 3)` 应该返回 `"***"`.

`repeat("abc", 3)` 应该返回 `"abcabcabc"`.

`repeat("abc", 4)` 应该返回 `"abcabcabcabc"`.

`repeat("abc", 1)` 应该返回 `"abc"`.

`repeat("*", 8)` 应该返回 `"********"`.

`repeat("abc", -2)` 应该返回 `""`.