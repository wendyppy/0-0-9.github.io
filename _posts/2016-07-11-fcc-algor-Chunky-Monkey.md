---
layout: post
title: "Chunky Monkey(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","算法基础","algorithm"]
---
{% include JB/setup %}

###题目

猴子吃香蕉可是掰成好几段来吃哦！

把一个数组arr按照指定的数组大小size分割成若干个数组块。

例如:`chunk([1,2,3,4],2)=[[1,2],[3,4]]`;

`chunk([1,2,3,4,5],2)=[[1,2],[3,4],[5]]`;

###提示

[Array.push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

[Array.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

###思路

数组分割成块，看到这个就想到`slice()`方法。`slice()`可接收两个参数，返回项的起始位置、（可选）返回项的结束位置。准确的说，返回的数组是从第一个参数到第二个参数前一个位置的项，也就是说，返回的数组包含起始位置但不包含结束位置。

用`for()`循环迭代传进来的数组，在循环内部用把`slice()`切割的块推入新声明的空数组中，就能达到预期结果啦！

###解法

<pre>
function chunk(arr, size) {
  // Break it up.
  var temp = [];  
  for(var i = 0; i < arr.length; i+=size){
    temp.push(arr.slice(i,i+size));
  }  
  return temp;
}
</pre>

###测试

`chunk(["a", "b", "c", "d"], 2)` 应该返回 `[["a", "b"], ["c", "d"]]`.

`chunk([0, 1, 2, 3, 4, 5], 3)` 应该返回 `[[0, 1, 2], [3, 4, 5]]`.

`chunk([0, 1, 2, 3, 4, 5], 2)` 应该返回 `[[0, 1], [2, 3]`, `[4, 5]]`.

`chunk([0, 1, 2, 3, 4, 5], 4)` 应该返回 `[[0, 1, 2, 3], [4, 5]]`.

`chunk([0, 1, 2, 3, 4, 5, 6], 3)` 应该返回 `[[0, 1, 2], [3, 4, 5], [6]]`.

`chunk([0, 1, 2, 3, 4, 5, 6, 7, 8], 4)` 应该返回 `[[0, 1, 2, 3], [4, 5, 6, 7], [8]]`.
