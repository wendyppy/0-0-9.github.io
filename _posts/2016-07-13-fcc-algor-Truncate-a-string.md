---
layout: post
title: "Truncate a string(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","String"]
---
{% include JB/setup %}

###题目

用瑞兹来截断对面的退路!

截断一个字符串！

如果字符串的长度比指定的参数num长，则把多余的部分用...来表示。

切记，插入到字符串尾部的三个点号也会计入字符串的长度。

但是，如果指定的参数num小于或等于3，则添加的三个点号不会计入字符串的长度。

###提示

[String.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice)

###思路

本题考查字符串截断的基本应用，按题目顺序解答即可。

###解法

<pre>
function truncate(str, num) {
  // Clear out that junk in your trunk
  if(str.length > num){
    if(num <= 3){
      str = str.slice(0,num) + "...";
    }else{
      str = str.slice(0,num-3) + "...";
    }    
  }
  return str;
}
</pre>

从结果上来看，把代码中的`slice`替换为`substr`也是可行的。

###测试

`truncate("A-tisket a-tasket A green and yellow basket", 11)` 应该返回 "A-tisket...".

`truncate("Peter Piper picked a peck of pickled peppers", 14)` 应该返回 "Peter Piper...".

`truncate("A-tisket a-tasket A green and yellow basket", "A-tisket a-tasket A green and yellow basket".length)` 应该返回 "A-tisket a-tasket A green and yellow basket".

`truncate("A-tisket a-tasket A green and yellow basket", "A-tisket a-tasket A green and yellow basket".length + 2)` 应该返回 "A-tisket a-tasket A green and yellow basket".

`truncate("A-", 1)` 应该返回 "A...".

`truncate("Absolutely Longer", 2)` 应该返回 "Ab...".