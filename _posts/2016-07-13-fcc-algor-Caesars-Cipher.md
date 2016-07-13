---
layout: post
title: "Caesars Cipher(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

让上帝的归上帝，凯撒的归凯撒。

下面我们来介绍风靡全球的凯撒密码Caesar cipher，又叫移位密码。

移位密码也就是密码中的字母会按照指定的数量来做移位。

一个常见的案例就是ROT13密码，字母会移位13个位置。由'A' ↔ 'N', 'B' ↔ 'O'，以此类推。

写一个ROT13函数，实现输入加密字符串，输出解密字符串。

所有的字母都是大写，不要转化任何非字母形式的字符(例如：空格，标点符号)，遇到这些特殊字符，跳过它们。

###提示

[String.charCodeAt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt)

[String.fromCharCode()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode)

###思路

本题的核心在于提示里给出的两个方法，我们来分别了解一下。

**String.prototype.charCodeAt()**

语法：

><pre>str.charCodeAt(index)</pre>

示例：

<pre>"ABC".charCodeAt(0) // returns 65</pre>

上例返回 65，即 A 的 Unicode 值。

**String.fromCharCode()**

语法：

><pre>String.fromCharCode(num1, ..., numN)</pre>

该方法返回一个字符串，而不是一个 String 对象。

由于 fromCharCode 是 String 的静态方法，所以应该像这样使用：String.fromCharCode()，而不是作为你创建的 String 对象的方法。

示例：

<pre>String.fromCharCode(65,66,67) // returns "ABC"</pre>

好了，回到题目。传入的字符串都是大写，而大写字母 A 到 Z 的 Unicode 值是升序排列的。加密算法的核心是前13个字母 Unicode 值加13，后13个字母 Unicode 值减13从字母表重新回滚。而其他大写字母以外的空白符符号等等字符原样不变。

###解法

<pre>
function rot13(str) { // LBH QVQ VG!
  var index = null;
  var temp = "";
  var _A = "A".charCodeAt(0);
  var _Z = "Z".charCodeAt(0);
  var mid = (_A + _Z)/2;
  for(var i = 0; i < str.length; i++){
    index = str.charCodeAt(i);
    if(index >= _A && index <= mid ){
      temp += String.fromCharCode(index + 13);
    }else if(index <= _Z && index > mid ){
      temp += String.fromCharCode(index - 13);
    }else{
      temp += String.fromCharCode(index);
    }   
  }
  return temp;
}
</pre>

###测试

