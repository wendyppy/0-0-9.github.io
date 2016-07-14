---
layout: post
title: "Spinal Tap Case(算法)"
keywords: ["FCC","算法","freecodecamp","Spinal Tap Case"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","中级算法","algorithm","RegExp"]
---
{% include JB/setup %}

###题目

将字符串转换为 spinal case。Spinal case 是 all-lowercase-words-joined-by-dashes 这种形式的，也就是以连字符连接所有小写单词。

###提示

[RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[String.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

###思路

整体思路就是用正则表达式替换字符串指定字符。难点在于正则的写法。现逐步分析。

观察第三个测试语句，发现有的单词之间用下划线`"_"`连接，其它用空格连接，需要把它们转换成同一种占位类型，方便最后统一转换成连接符`"-"`。这里先把下划线转为空格。

<pre>
str = str.replace(/_/g," ")
</pre>

测试`spinalCase("The_Andy_Griffith_Show")`，结果为：

<pre>
The Andy Griffith Show
</pre>

观察第二个测试语句，单词之间并无空格或其他连接符，而是以首字母大写作为单个单词的判断标准。在大写字母之前添加空格，与上面代码保持一致。本解法中，所有单词分隔在转为`"-"`之前都用空格表示。

<pre>
str = str.replace(/([A-Z])/g," $1");
</pre>

其中，小括号表示分组，`$1`表示第1个小括号捕获内容。

测试`spinalCase("thisIsSpinalTap")`，结果为：

<pre>
this Is Spinal Tap
</pre>

然而，这样做之后，有的语句因为首单词的首字符也是大写的，所以前面也会多个空格。我们需要排除这种情况：

<pre>
str = str.replace(/^\s/,"");
</pre>

其中`"^"`表示语句开头，`"\s"`表示空白符。

接下来，可以把所有空白符替换为题目要求的连接符`"-"`：

<pre>
str = str.replace(/\s+/g,"-");
</pre>

其中，`"+"`表示匹配前一项一或多次，如果不加这个，有一个以上空格的地方会同时出现多个`"-"`并用情况。

最后，把所有字符转为小写，完成！

###解法

<pre>
function spinalCase(str) {
  // "It's such a fine line between stupid, and clever."
  // --David St. Hubbins
  str = str.replace(/_/g," ")
        .replace(/([A-Z])/g," $1")
        .replace(/^\s/,"")
        .replace(/\s+/g,"-")
        .toLowerCase();
  return str;
}
</pre>

###测试

`spinalCase("This Is Spinal Tap")` 应该返回 "this-is-spinal-tap"。

`spinalCase("thisIsSpinalTap")` 应该返回 "this-is-spinal-tap"。

`spinalCase("The_Andy_Griffith_Show")` 应该返回 "the-andy-griffith-show"。

`spinalCase("Teletubbies say Eh-oh")` 应该返回 "teletubbies-say-eh-oh"。

###在线调试

[Spinal Tap Case](https://freecodecamp.cn/challenges/spinal-tap-case)