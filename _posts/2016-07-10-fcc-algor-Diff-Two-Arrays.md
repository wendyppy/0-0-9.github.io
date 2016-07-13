---
layout: post
title: "Diff Two Arrays(算法)"
keywords: ["FCC ","算法","freecodecamp","Diff Two Arrays"]
description: "FCC算法题 Diff Two Arrays"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Array"]
---
{% include JB/setup %}

###题目

比较两个数组，然后返回一个新数组，该数组的元素为两个给定数组中所有独有的数组元素。换言之，返回两个数组的差异。

###提示

[Comparison Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

[Array.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

[Array.filter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

[Array.indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

[Array.concat()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)

###思路

寻找独有的数组元素，第一反应就是数组的迭代方法`filter()`方法。对传入的两个数组`a`、`b`，可以先选出`a`中`b`所没有的元素，然后再选出`b`中`a`所没有的元素，最后用`concat()`方法将这二者拼接起来。

**filter()相关知识：**

数组的5种迭代方法（即`every()`，`filter()`，`forEach()`，`map()`，`some()`）可以接收两个参数：

1 ) 要在每一项上运行的函数。
   该函数可以接收三个参数：数组项的值、该项在数组中的位置、数组对象本身。

2 ) （可选）运行该函数的作用域对象，即`this`.

语法：

<pre>
arr.filter(callback[, thisArg])
</pre>

需要知道的是，`filter()`不会改变原数组。

**位置方法indexOf()：**

`indexOf()`是从头至尾查找，它接收两个参数：要查找的项、（可选）表示查找起点位置的索引。

和它很相像的是从尾至头查找的`lastIndexOf()`方法。这两个方法都返回要查找的项在数组中的位置，没找到的话就返回`-1`。

需要注意的是，用`indexOf()`比较第一个参数和数组中的每一项时，会使用全等操作符`===`。如果有两个对象，它们的`key`、`value`都一样，也是不能被判断成相等的。因为基本类型通过值来比较，而对象是通过指向的内存中的地址来比较的。本题解答中不考虑具有相同`key`、`value`的对象会相等的情况。

###解法

<pre>
function diff(a, b)
{
  var a1 = a.filter(function(item,index,array){
    return (b.indexOf(item) < 0);
  });
  var b1 = b.filter(function(item,index,array){
    return (a.indexOf(item) < 0);
  });
  return a1.concat(b1);
}
</pre>

###测试

`diff([1, 2, 3, 5], [1, 2, 3, 4, 5])` 应该返回一个数组。

`["diorite", "andesite", "grass", "dirt", "pink wool", "dead shrub"], ["diorite", "andesite", "grass", "dirt", "dead shrub"]` 应该返回 `["pink wool"]`。

`["andesite", "grass", "dirt", "pink wool", "dead shrub"], ["diorite", "andesite", "grass", "dirt", "dead shrub"]` 应该返回 `["diorite", "pink wool"]`。

`["andesite", "grass", "dirt", "dead shrub"], ["andesite", "grass", "dirt", "dead shrub"]` 应该返回 `[]`。

`[1, 2, 3, 5], [1, 2, 3, 4, 5]` 应该返回 `[4]`。

`[1, "calf", 3, "piglet"], [1, "calf", 3, 4]` 应该返回 `["piglet", 4]`。

`[], ["snuffleupagus", "cookie monster", "elmo"]` 应该返回 `["snuffleupagus", "cookie monster", "elmo"]`。

`[1, "calf", 3, "piglet"], [7, "filly"]` 应该返回 `[1, "calf", 3, "piglet", 7, "filly"]`。