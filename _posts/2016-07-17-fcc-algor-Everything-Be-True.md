---
layout: post
title: "Everything Be True(算法)"
keywords: ["FCC","算法","freecodecamp","Everything Be True"]
description: "FCC算法题 Everything Be True"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

判断谓词（第二个参数）是否对一个集合（第一个参数）的所有元素都成立。

记住，你可以通过点号或方括号来访问对象的属性。

###提示

无

###思路

这里用到了一个数组的迭代方法`every()`。该方法将传入一个函数，当每一项均返回 true 时，该函数才返回 true。

###解法

<pre>
function every(collection, pre) {
  // Is everyone being true?
  return collection.every(function(item,index,array){
    return item[pre];
  });
}
</pre>

###测试

`every([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}, {"user": "Laa-Laa", "sex": "female"}, {"user": "Po", "sex": "female"}], "sex")` 应该返回 true。

`every([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy"}, {"user": "Laa-Laa", "sex": "female"}, {"user": "Po", "sex": "female"}], "sex")` 应该返回 false。

`every([{"user": "Tinky-Winky", "sex": "male", "age": 0}, {"user": "Dipsy", "sex": "male", "age": 3}, {"user": "Laa-Laa", "sex": "female", "age": 5}, {"user": "Po", "sex": "female", "age": 4}], "age")` 应该返回 false。

`every([{"name": "Pete", "onBoat": true}, {"name": "Repeat", "onBoat": true}, {"name": "FastFoward", "onBoat": null}], "onBoat")` 应该返回 false

`every([{"name": "Pete", "onBoat": true}, {"name": "Repeat", "onBoat": true, "alias": "Repete"}, {"name": "FastFoward", "onBoat": true}], "onBoat")` 应该返回 true

`every([{"single": "yes"}], "single")` 应该返回 true

`every([{"single": ""}, {"single": "double"}], "single")` 应该返回 false

`every([{"single": "double"}, {"single": undefined}], "single")` 应该返回 false

`every([{"single": "double"}, {"single": NaN}], "single")` 应该返回 false

###在线调试

[Everything Be True](https://freecodecamp.cn/challenges/everything-be-true)