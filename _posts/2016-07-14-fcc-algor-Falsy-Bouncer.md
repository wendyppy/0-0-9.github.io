---
layout: post
title: "Falsy Bouncer(算法)"
keywords: ["FCC","算法","freecodecamp","Falsy Bouncer"]
description: "FCC算法题 Falsy Bouncer"
category: "FCC"
tags: ["FCC","算法基础","algorithm"]
---
{% include JB/setup %}

###题目

真假美猴王！

删除数组中的所有假值。

在JavaScript中，假值有false、null、0、""、undefined 和 NaN。

###提示

[Boolean Objects](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

[Array.filter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

###思路

我们可以用`Boolean()`函数进行类型转换。如果它的参数是 0、-0、null、undefined、false、NaN、""，生成的Boolean对象的值会为false，也就是题目中说的“假值”。

###解法

**解法一**

<pre>
function bouncer(arr) {
  // Don't show a false ID to this bouncer.
  return arr.filter(function(item,index,array){
    return Boolean(item);
  });
}
</pre>

**解法二**

<pre>
function bouncer(arr) {
  // Don't show a false ID to this bouncer.
  return arr.filter(Boolean);
}
</pre>

###测试

`bouncer([7, "ate", "", false, 9])` 应该返回 [7, "ate", 9].

`bouncer(["a", "b", "c"])` 应该返回 ["a", "b", "c"].

`bouncer([false, null, 0, NaN, undefined, ""])` 应该返回 [].

`bouncer([1, null, NaN, 2, undefined])` 应该返回 [1, 2].
