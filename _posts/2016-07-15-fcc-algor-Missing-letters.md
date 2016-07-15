---
layout: post
title: "Missing letters(算法)"
keywords: ["FCC","算法","freecodecamp","Missing letters"]
description: "FCC算法题 Missing letters"
category: "FCC"
tags: ["FCC","中级算法","algorithm","String"]
---
{% include JB/setup %}

###题目

从传递进来的字母序列中找到缺失的字母并返回它。

如果所有字母都在序列中，返回 undefined。

###提示

[String.charCodeAt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

[String.fromCharCode()
](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

###思路

如果传进来的字母序列只漏掉一个字符的话，这道题目还是很容易解决的，不用很多代码量就能验证通过。但是，题面说的是“找到缺失的字母”而不是“找到缺失的单个字母”，所以出于解题的完整性考虑，即使测试题目中验证的也都是缺失单个字符的情况，我们还是用数组解决它，使得即使缺失了多个字符也能返回正确结果。

把传入的字母序列转化为数组；声明空数组，准备推入值作为最后的返回结果：

<pre>
var arr = str.split("");
var temp = [];
</pre>

假设传入的是一个字母序列连续的字符串，那么它的每个字符的 unicode 值应当是依次递增 1 的。

<pre>
//字符串首位字符编码
var start = str.charCodeAt(0);
//字符串末尾字符编码
var end = str.charAt(str.length - 1).charCodeAt(0);
for(var i = start; i < end + 1; i++){
    //do something...
  }
</pre>

解释一下这个`for()`循环的条件。这里把传入的 str 当做一个字母序列连续的字符串看待。`i`代表当前字符的字符编码，即 unicode 值。

我们来看一下循环内部应该进行的操作：

<pre>
var item = String.fromCharCode(i);
    if(arr[0] !== item){  
      temp.push(item);
    }else{
      arr.shift();
    }
</pre>

`item`是当前字符串是 unicode 值依次递增的连续字符串的情况下，当前项的值。

如果当前项的值就是字符串首项，说明它不是我们要找的值，移除str转化的数组arr的首项；如果它不是字符串首项，说明这个位置本应该出现的值被其它值顶替，把这个位置本应该出现的值收入将要返回的结果集中。

整个函数的返回值需做以下判断：

<pre>
if(temp.length === 0){
    return undefined;
  }else{
    return temp.join("");
  }  
</pre>

###解法

<pre>
function fearNotLetter(str) {
  var arr = str.split("");
  var temp = [];
  var start = str.charCodeAt(0);
  var end = str.charAt(str.length - 1).charCodeAt(0);
  for(var i = start; i < end + 1; i++){
    var item = String.fromCharCode(i);
    if(arr[0] !== item){  
      temp.push(item);
    }else{
      arr.shift();
    }
  }
  if(temp.length === 0){
    return undefined;
  }else{
    return temp.join("");
  }  
}
</pre>

我们自己来测试一下吧：

`fearNotLetter("bcfg")` 返回 "de"，成功。

###测试

`fearNotLetter("abce")` 应该返回 "d"。

`fearNotLetter("abcdefghjklmno")` 应该返回 "i"。

`fearNotLetter("bcd")` 应该返回 undefined。

`fearNotLetter("yz")` 应该返回 undefined。

###在线调试

[Missing letters](https://freecodecamp.cn/challenges/missing-letters)