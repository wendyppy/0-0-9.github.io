---
layout: post
title: "Search and Replace(算法)"
keywords: ["FCC","算法","freecodecamp","Search and Replace"]
description: "FCC算法题 Search and Replace"
category: "FCC"
tags: ["FCC","中级算法","algorithm","String"]
---
{% include JB/setup %}

###题目

使用给定的参数对句子执行一次查找和替换，然后返回新句子。

第一个参数是将要对其执行查找和替换的句子。

第二个参数是将被替换掉的单词（替换前的单词）。

第三个参数用于替换第二个参数（替换后的单词）。

注意：替换时保持原单词的大小写。例如，如果你想用单词 "dog" 替换单词 "Book" ，你应该替换成 "Dog"。

###提示

[Array.splice()
](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

[String.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

[Array.join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

###思路

这题很明显要使用字符串的`replace()`方法。

`String.prototype.replace()`语法：

><pre>str.replace(regexp|substr, newSubStr|function)</pre>

余下的问题在于如何替换时保持原单词的大小写。这个指的应当是单词的首字母。

**解法一**

索引方式访问字符串。JavaScript中可以用索引方式访问单个字符，就像访问数组中的某一项一样。

<pre>
if(before[0] === before[0].toUpperCase()){
    //do something...
  }
</pre>

**解法二**

用正则表达式匹配；用`charAt()`访问单个字符。

使用正则表达式的好处在于，即使未来替换规则改变，代码修改量也不会很大。

<pre>
var re = /^[A-Z]/;
if(re.test(before.charAt(0))){
    //do something...
  }
</pre>

这里顺便说一下访问单个字符时，`str[index]`和`charAt(index)`的区别：

对于**超出范围的索引值**，`str[index]`将返回 undefined ，而`charAt(index)`将返回一个空字符串。

对于**IE8以下版本**，`str[index]`不被兼容，将返回 undefined。

###解法

**解法一**

<pre>
function myReplace(str, before, after) {
  if(before[0] === before[0].toUpperCase()){
    after = after[0].toUpperCase() + after.slice(1);
  }
  str = str.replace(before,after);  
  return str;
}
</pre>

**解法二**

<pre>
function myReplace(str, before, after) {
  var re = /^[A-Z]/;
  if(re.test(before.charAt(0))){
    after = after.charAt(0).toUpperCase() + after.slice(1);
  }
  str = str.replace(before,after);
  return str;
}
</pre>

###测试

`myReplace("Let us go to the store", "store", "mall")` 应该返回 "Let us go to the mall"。

`myReplace("He is Sleeping on the couch", "Sleeping", "sitting")` 应该返回 "He is Sitting on the couch"。

`myReplace("This has a spellngi error", "spellngi", "spelling")` 应该返回 "This has a spelling error"。

`myReplace("His name is Tom", "Tom", "john")` 应该返回 "His name is John"。

`myReplace("Let us get back to more Coding", "Coding", "algorithms")` 应该返回 "Let us get back to more Algorithms"。
