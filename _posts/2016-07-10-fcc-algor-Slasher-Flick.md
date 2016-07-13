---
layout: post
title: "Slasher Flick(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Array"]
---
{% include JB/setup %}

###题目

打不死的小强！

返回一个数组被截断n个元素后还剩余的元素，截断从索引0开始。

###提示

[Array.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

[Array.splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

###思路

既然题目已经给了提示可以用到的函数，那么我们就可以分别用这两种函数得出两种解题思路。

**解法一**

`slice()`方法会基于当前数组中的一个或多个项创建一个新数组。可以接受一或两个参数，即返回项的起始和结束位置。 <span class="txt">[起始,结束)</span>

`slice()`不会影响原数组。

用以下代码，问题可简单解决：

<pre>
arr.slice(howMany)
</pre>

因为数组项的索引从0开始，所以起始位置可设为截断项`howMany`，不指定第二个参数，则结束位置一直到整个数组末尾。

**解法二**

`splice()`是个很灵活也很强大的方法，用它可以在数组中进行数组项的删除、插入、替换。它可以接收三个参数：<span class="txt">起始位置</span>、<span class="txt">要删除的项数</span>、<span class="txt">要插入的任意数量的项</span>。

`splice()`方法始终会返回一个数组，该数组包含从原数组中删除的项（若没删除任何值，则返回空数组）。这个方法会修改原数组。

如果想在`return`时直接返回题目要求的`一个数组被截断n个元素后还剩余的元素`，则可以用`splice()`删除最后要得到的值。当然啦，从逻辑上来说，这样写与题意是偏离的（题中要求保留剩余的元素，而我们修改数组并删除了这部分），但从返回结果上来看是正确的。

###解法

**解法一**

<pre>
function slasher(arr, howMany) {
  // it doesn't always pay to be first
  return arr.slice(howMany);
}
</pre>

**解法二**

<pre>
function slasher(arr, howMany) {
  // it doesn't always pay to be first  
  return arr.splice(howMany,arr.length);
}
</pre>

###测试

`slasher([1, 2, 3], 2)` 应该返回 `[3]`.

`slasher([1, 2, 3], 0)` 应该返回 `[1, 2, 3]`.

`slasher([1, 2, 3], 9)` 应该返回 `[]`.

`slasher([1, 2, 3], 4)` 应该返回 `[]`.

`slasher(["burgers", "fries", "shake"], 1)` 应该返回 `["fries", "shake"]`.