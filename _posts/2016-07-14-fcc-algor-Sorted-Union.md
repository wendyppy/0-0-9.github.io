---
layout: post
title: "Sorted Union(算法)"
keywords: ["FCC","算法","freecodecamp","Sorted Union"]
description: "FCC算法题 Sorted Union"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

写一个 function，传入两个或两个以上的数组，返回一个以给定的原始数组排序的不包含重复值的新数组。

换句话说，所有数组中的所有值都应该以原始顺序被包含在内，但是在最终的数组中不包含重复值。

非重复的数字应该以它们原始的顺序排序，但最终的数组不应该以数字顺序排序。

请参照下面验证判断中的例子。

###提示

[Arguments object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)

[Array.reduce()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

###思路

主干思路很清晰，先拼接数组，再去重。

先把所有参数拼接成数组，这需要用到 Array 的归并方法`reduce()`。但传进来的参数 arguments 只是类数组对象，我们首先要把它转化为真正的数组：

<pre>var args = Array.from(arguments);</pre>

然后得到一个拼接而成的大数组：

<pre>var arr = args.reduce(function(prev,cur,index,array){
    return prev.concat(cur);
  });</pre>
  
最后用`filter()`方法去重：

<pre>
arr.filter(function(item,index,array){
    return array.indexOf(item) === index;
  });
</pre>

###解法

<pre>
function unite(arr1, arr2, arr3) {
  var args = Array.from(arguments);
  var arr = args.reduce(function(prev,cur,index,array){
    return prev.concat(cur);
  });
  return arr.filter(function(item,index,array){
    return array.indexOf(item) === index;
  });
}
</pre>

###测试

`unite([1, 3, 2], [5, 2, 1, 4], [2, 1])` 应该返回 [1, 3, 2, 5, 4]。

`unite([1, 3, 2], [1, [5]], [2, [4]])` 应该返回 [1, 3, 2, [5], [4]]。

`unite([1, 2, 3], [5, 2, 1])` 应该返回 [1, 2, 3, 5]。

`unite([1, 2, 3], [5, 2, 1, 4], [2, 1], [6, 7, 8])` 应该返回 [1, 2, 3, 5, 4, 6, 7, 8]。

###在线调试

[Sorted Union](https://freecodecamp.cn/challenges/sorted-union)