---
layout: post
title: "Title Case a Sentence(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

确保字符串的每个单词首字母都大写，其余部分小写。

像'the'和'of'这样的连接符同理。

###提示

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

###思路

把传入的字符串转化为数组，对数组每一项进行遍历，按题意要求的操作后再拼接成字符串。

下面解法中出于效率考虑，更推荐第一种。因为`map()`底层也是由循环实现的。

###解法

**解法一**

<pre>
function titleCase(str) {
  var arr = str.toLowerCase().split(" ");
  for(var i = 0; i < arr.length; i++){
    arr[i] = arr[i].charAt(0).toUpperCase() + arr[i].slice(1);
  }
  return arr.join(" ");
}
</pre>

**解法二**

<pre>
function titleCase(str) {
  var arr = str.toLowerCase().split(" ");
  str = arr.map(function(item,index,array){  
    return item.charAt(0).toUpperCase() + item.slice(1);
  }).join(" ");
  return str;
}
</pre>

###测试

`titleCase("I'm a little tea pot")` 应该返回一个字符串

`titleCase("I'm a little tea pot")` 应该返回 "I'm A Little Tea Pot".

`titleCase("sHoRt AnD sToUt")` 应该返回 "Short And Stout".

`titleCase("HERE IS MY HANDLE HERE IS MY SPOUT")` 应该返回 "Here Is My Handle Here Is My Spout".