---
layout: post
title: "Pig Latin(算法)"
keywords: ["FCC","算法","freecodecamp","Pig Latin"]
description: "FCC算法题 Pig Latin"
category: "FCC"
tags: ["FCC","中级算法","algorithm","String"]
---
{% include JB/setup %}

###题目

把指定的字符串翻译成 pig latin。

Pig Latin 把一个英文单词的第一个辅音或辅音丛（consonant cluster）移到词尾，然后加上后缀 "ay"。

如果单词以元音开始，你只需要在词尾添加 "way" 就可以了。

###提示

[Array.indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

[Array.push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

[Array.join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

[String.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

###思路

英文中的元音字母有：`"a","e","i","o","u"`，把它们放进一个数组中，看其中有没有与字符串首字符相同的项。以此为基础，进行判断。

###解法

<pre>
function translate(str) {
  var yuan = ["a","e","i","o","u"];
  if(yuan.indexOf(str[0]) >= 0){
    return str + "way";
  }
  while(yuan.indexOf(str[0]) < 0){
    str = str.substr(1) + str.substr(0,1);
  }
  return str + "ay";
}
</pre>

###测试

`translate("california")` 应该返回 "aliforniacay"。

`translate("paragraphs")` 应该返回 "aragraphspay"。

`translate("glove")` 应该返回 "oveglay"。

`translate("algorithm")` 应该返回 "algorithmway"。

`translate("eight")` 应该返回 "eightway"。

###在线调试

[Pig Latin](https://freecodecamp.cn/challenges/pig-latin)