---
layout: post
title: "Drop it(算法)"
keywords: ["FCC","算法","freecodecamp","Drop it"]
description: "FCC算法题 Drop it"
category: "FCC"
tags: ["FCC","中级算法","algorithm"]
---
{% include JB/setup %}

###题目

从数组（第一个参数）的最前面开始丢弃其中的元素，直到谓词（第二个参数）返回 true。

返回数组剩余的部分，如果没有剩余元素则返回一个空数组。

###提示

[Arguments object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)

[Array.shift()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)

[Array.slice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

###思路

用`while()`循环，当`func(arr[0])`不被满足时移除arr首项。然后继续遍历数组剩余部分。

要注意的是，当`func(arr[0])`一直不被满足时，arr首项将被无限多（浏览器最大值边界）次移除，即使arr为空数组也会循环此操作。为了保证效率，应加上限制条件`arr.length > 0`阻止无限循环的发生。

###解法

<pre>
function drop(arr, func) {
  // Drop them elements.
  while(!func(arr[0]) && arr.length > 0){
    arr.shift();
  }
  return arr;
}
</pre>

###测试

drop([1, 2, 3, 4], function(n) {return n >= 3;}) 应该返回 [3, 4]。

drop([0, 1, 0, 1], function(n) {return n === 1;}) 应该返回 [1, 0, 1]。

drop([1, 2, 3], function(n) {return n > 0;}) 应该返回 [1, 2, 3]。

drop([1, 2, 3, 4], function(n) {return n > 5;}) 应该返回 []。

drop([1, 2, 3, 7, 4], function(n) {return n > 3;}) 应该返回 [7, 4]。

drop([1, 2, 3, 9, 2], function(n) {return n > 2;}) 应该返回 [3, 9, 2]。

###在线调试

[Drop it](https://freecodecamp.cn/challenges/drop-it)