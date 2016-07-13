---
layout: post
title: "Find the Longest Word in a String(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

找到提供的句子中最长的单词，并计算它的长度。

函数的返回值应该是一个数字。

###提示

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

[String.length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/length)

###思路

用空格切割字符串成单词数组，遍历它并选出length最长的项。

###解法

<pre>
function findLongestWord(str) {
  var arr = str.split(" ");
  var temp = null;
  for(var i = 0; i < arr.length; i++){
    if(arr[i].length > temp){
      temp = arr[i].length;
    }
  }
  return temp;
}
</pre>

###测试

`findLongestWord("The quick brown fox jumped over the lazy dog")` 应该返回一个数字

`findLongestWord("The quick brown fox jumped over the lazy dog")` 应该返回 6.

`findLongestWord("May the force be with you")` 应该返回 5.

`findLongestWord("Google do a barrel roll")` 应该返回 6.

`findLongestWord("What is the average airspeed velocity of an unladen swallow")` 应该返回 8.

`findLongestWord("What if we try a super-long word such as otorhinolaryngology")` 应该返回 19.
