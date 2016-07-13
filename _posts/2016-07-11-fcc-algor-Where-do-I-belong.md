---
layout: post
title: "Where do I belong(算法)"
keywords: ["FCC ","算法","freecodecamp","Where do I belong"]
description: "FCC算法题 Where do I belong"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Array"]
---
{% include JB/setup %}

###题目

我身在何处？

先给数组排序，然后找到指定的值在数组的位置，最后返回位置对应的索引。

举例：<code class="txt">where([1,2,3,4], 1.5)</code> 应该返回 <code class="txt">1</code>。因为<code class="txt">1.5</code>插入到数组<code class="txt">[1,2,3,4]</code>后变成<code class="txt">[1,1.5,2,3,4]</code>，而<code class="txt">1.5</code>对应的索引值就是1。

同理，<code class="txt">where([20,3,5], 19)</code> 应该返回 <code class="txt">2</code>。因为数组会先排序为 <code class="txt">[3,5,20]</code>，<code class="txt">19</code>插入到数组<code class="txt">[3,5,20]</code>后变成<code class="txt">[3,5,19,20]</code>，而<code class="txt">19</code>对应的索引值就是<code class="txt">2</code>。

###提示

[Array.sort()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

###思路

先把指定的值加入数组，然后升序排序数组，最后用`indexOf()`方法返回传入值在数组中的位置。

###解法

<pre>
function where(arr, num) {
  // Find my place in this sorted array.
  arr.push(num);
  arr.sort(function(v1,v2){
    return v1 - v2;
  });
  return arr.indexOf(num);
}
</pre>

###测试

`where([10, 20, 30, 40, 50], 35)` 应该返回 `3`.

`where([10, 20, 30, 40, 50], 30)` 应该返回 `2`.

`where([40, 60], 50)` 应该返回 `1`.

`where([3, 10, 5], 3)` 应该返回 `0`.

`where([5, 3, 20, 3], 5)` 应该返回 `2`.

`where([2, 20, 10], 19)` 应该返回 `2`.

`where([2, 5, 10], 15)` 应该返回 `3`.