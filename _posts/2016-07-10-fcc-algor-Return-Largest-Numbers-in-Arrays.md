---
layout: post
title: "Return Largest Numbers in Arrays(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm"]
---
{% include JB/setup %}

###题目

右边大数组中包含了4个小数组，分别找到每个小数组中的最大值，然后把它们串联起来，形成一个新数组。

提示：你可以用for循环来迭代数组，并通过arr[i]的方式来访问数组的每个元素。
###提示

[Comparison Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

###思路

用`for()`循环遍历可得到大数组中的每一项小数组。

**解法一**

看到“找到最大值”首先想到的当然是`sort()`方法。但`sort()`在排序时会调用数组每一项的`toString()`方法，比较的是字符串。要实现题目中比较数值的要求，我们可以给`sort()`传递一个降序排列的比较函数：
<pre>
//降序排列
function compare(v1,v2){
  return v2 - v1;
}
</pre>
`sort()`返回的是重排序后的数组，是对原数组进行修改，所以可以不必用新变量接收修改后的数组。
对重排序后的数组来说，因为是降序排列，所以每小组中的最大值就是第一项。

声明一个空数组，在循环体内把每个小数组的最大值推入，就是最后想要的返回结果啦！

**解法二**

对找小数组中最大值的遍历，有人提出了`sort()`的效率问题（感谢 [@saymagic](http://blog.saymagic.cn/) 的宝贵意见😊 ），所以补充一下用`reduce()`做的更优解。

`redece()`是ES5增加的归并数组的方法之一，它会迭代数组所有项，并构建一个最终返回的值。

保持原思路不变，把解法一中关于小数组排序的模块替换为以下代码：

<pre>
var l = arr[i].reduce(function(prev,cur,index,array){
      return prev > cur ? prev : cur;
    });
</pre>

其中，`l`代表小数组中的最大值，在循环中将它推入新声明的空数组，就能得到想要的结果！

###解法

**解法一**

<pre>
function largestOfFour(arr) {
  // You can do this!
  var temp = [];
  for (var i = 0; i < arr.length; i++){
    arr[i].sort(function(v1,v2){
    	return v2 - v1;
    });
    temp.push(arr[i][0]);
  }
  return temp;
}
</pre>

**解法二**

<pre>
function largestOfFour(arr) {
  // You can do this!
  var temp = [];
  for(var i = 0; i < arr.length; i++){
    var l = arr[i].reduce(function(prev,cur,index,array){
      return prev > cur ? prev : cur;
    });
    temp.push(l);
  }
  return temp;
}
</pre>
###测试

<span class="txt">largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]) </span>应该返回一个数组

<span class="txt">largestOfFour([[13, 27, 18, 26], [4, 5, 1, 3], [32, 35, 37, 39], [1000, 1001, 857, 1]]) </span>应该返回<span class="txt"> [27,5,39,1001]</span>.

<span class="txt">largestOfFour([[4, 9, 1, 3], [13, 35, 18, 26], [32, 35, 97, 39], [1000000, 1001, 857, 1]]) </span>应该返回<span class="txt"> [9, 35, 97, 1000000]</span>.