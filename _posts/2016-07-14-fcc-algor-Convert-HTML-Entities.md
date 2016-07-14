---
layout: post
title: "Convert HTML Entities"
keywords: ["FCC","算法","freecodecamp","Convert HTML Entities"]
description: "FCC算法题 Convert HTML Entities"
category: "FCC"
tags: ["FCC","算法基础","algorithm","RegExp"]
---
{% include JB/setup %}

###题目

将字符串中的字符 &、<、>、" （双引号）, 以及 ' （单引号）转换为它们对应的 HTML 实体。

如果你被卡住了，记得开大招 Read-Search-Ask 。试着与他人配对编程。编写你自己的代码。

###提示

[RegExp](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

[HTML Entities](https://dev.w3.org/html5/html-author/charref)

###思路

找到符号对应的HTML实体，用`replace()`替换即可。

###解法

<pre>
function convert(str) {
  // &colon;&rpar;
  str = str.replace(/[&]/g,"&amp;").replace(/[<]/g,"&lt;").replace(/[>]/g,"&gt;")
           .replace(/["]/g,"&quot;").replace(/[']/g,"&apos;");
  return str;
}
</pre>

###测试

`convert("Dolce & Gabbana")` 应该返回 Dolce &​amp; Gabbana。

`convert("Hamburgers < Pizza < Tacos")` 应该返回 Hamburgers &​lt; Pizza &​lt; Tacos。

`convert("Sixty > twelve")` 应该返回 Sixty &​gt; twelve。

`convert('Stuff in "quotation marks"')` 应该返回 Stuff in &​quot;quotation marks&​quot;。

`convert("Shindler's List")` 应该返回 Shindler&​apos;s List。

`convert("<>")` 应该返回 &​lt;&​gt;。

`convert("abc")` 应该返回 abc。

###在线调试

[Convert HTML Entities](https://freecodecamp.cn/challenges/convert-html-entities)