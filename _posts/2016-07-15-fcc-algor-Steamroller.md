---
layout: post
title: "Steamroller(算法)"
keywords: ["FCC","算法","freecodecamp","Steamroller"]
description: "FCC算法题 Steamroller"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

对嵌套的数组进行扁平化处理。你必须考虑到不同层级的嵌套。

###提示

[Array.isArray()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray)

###思路

`Array.isArray(value)`可以用来判断某个值是否为数组，是则返回`true`，不是返回`false`。

因为需要解决层级嵌套的问题，所以本题用递归来解决。

遍历 arr 的每一项，如果该项为数组，则重复调用本函数，如果不是数组，则将这一项推入结果集中。

###解法

<pre>
function steamroller(arr) {
  // I'm a steamroller, baby 
  var result = [];
  for(var i = 0; i < arr.length; i++){
    if(Array.isArray(arr[i])){
      result = result.concat(steamroller(arr[i]));
    }else{
      result.push(arr[i]);
    }
  }
  return result;
}
</pre>

###测试

`steamroller([[["a"]], [["b"]]])` 应该返回 ["a", "b"]。

`steamroller([1, [2], [3, [[4]]]])` 应该返回 [1, 2, 3, 4]。

`steamroller([1, [], [3, [[4]]]])` 应该返回 [1, 3, 4]。

`steamroller([1, {}, [3, [[4]]]])` 应该返回 [1, {}, 3, 4]。

###在线调试

[Steamroller](https://freecodecamp.cn/challenges/steamroller)