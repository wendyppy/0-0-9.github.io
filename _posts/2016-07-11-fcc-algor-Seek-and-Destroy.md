---
layout: post
title: "Seek and Destroy(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm","Array"]
---
{% include JB/setup %}

###题目

金克斯的迫击炮！

实现一个摧毁(destroyer)函数，第一个参数是待摧毁的数组，其余的参数是待摧毁的值。

###提示

[Arguments object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)

[]()

###思路

函数中的有隐式的不确定个数的参数，而我们在函数中将会用到它，很显然，这需要我们在`arguments`上做文章了。我们来看看MDN是怎么解释`arguments`的：
>arguments 是一个类数组对象。代表传给一个function的参数列表。

很显然，既然是类数组对象，说明它不是真正的数组，没有数组所具有的属性和方法（除了`length`属性）。为了便于我们接下来的运算，我们需要把arr后面的待摧毁的所有参数放进一个数组：

<pre>
var args = [];
for(var i = 1; i < arguments.length; i++){
  args.push(arguments[i]);
}
</pre>

拿到了待摧毁的项构成的数组，接下来就是用`filter()`方法实现的简单的去重操作了。

<pre>
arr.filter(function(item,index,array){
    return args.indexOf(item) < 0;
  });
</pre>

因为`filter()`不会改变原始数组，所以声明一个变量接收上一步的返回结果，最后整个函数返回这个变量，问题解决啦！

###解法

<pre>
function destroyer(arr) {
  // Remove all the values
  var args = [];
  for(var i = 1; i < arguments.length; i++){
    args.push(arguments[i]);
  }
  var temp = arr.filter(function(item,index,array){
    return args.indexOf(item) < 0;
  });
  return temp;
}
</pre>

###测试

`destroyer([1, 2, 3, 1, 2, 3], 2, 3)` 应该返回 `[1, 1]`.

`destroyer([1, 2, 3, 5, 1, 2, 3], 2, 3)` 应该返回 `[1, 5, 1]`.

`destroyer([3, 5, 1, 2, 2], 2, 3, 5)` 应该返回 `[1]`.

`destroyer([2, 3, 2, 3], 2, 3)` 应该返回 `[]`.

`destroyer(["tree", "hamburger", 53], "tree", 53)` 应该返回 `["hamburger"]`.
