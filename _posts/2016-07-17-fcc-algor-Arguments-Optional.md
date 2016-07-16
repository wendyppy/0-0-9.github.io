---
layout: post
title: "Arguments Optional(算法)"
keywords: ["FCC","算法","freecodecamp","Arguments Optional"]
description: "FCC算法题 Arguments Optional"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Closures"]
---
{% include JB/setup %}

###题目

创建一个计算两个参数之和的 function。如果只有一个参数，则返回一个 function，该 function 请求一个参数然后返回求和的结果。

例如，add(2, 3) 应该返回 5，而 add(2) 应该返回一个 function。

调用这个有一个参数的返回的 function，返回求和的结果：

var sumTwoAnd = add(2);

sumTwoAnd(3) 返回 5。

如果两个参数都不是有效的数字，则返回 undefined。

###提示

[Closures](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

[Arguments object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)

###思路

先让不符合条件的值返回 undefined：

<pre>
if(typeof arguments[0] !== "number" || 
	(arguments.length > 1 && typeof arguments[1] !== "number")){
    return undefined;
}
</pre>

本解法中，只有两种情况，一种是传入参数为一个，另一种是传入参数为两个（或以上）。

在第二种情况中，当传入参数为多个时，也只会返回前两个参数的和。

###解法

<pre>
function add() {
  if(typeof arguments[0] !== "number" || (arguments.length > 1 && typeof arguments[1] !== "number")){
    return undefined;
  }
  if(arguments.length == 1){
    var arg0 = arguments[0];
    return function(num){
      if(typeof num !== "number"){
        return undefined;
      }
      return arg0 + num;
    };
  }else{
    return arguments[0] + arguments[1];
  }
}
</pre>

###测试

`add(2, 3)` 应该返回 5。

`add(2)(3)` 应该返回 5。

`add("http://bit.ly/IqT6zt")` 应该返回 undefined。

`add(2, "3")` 应该返回 undefined。

`add(2)([3])` 应该返回 undefined。

###在线调试

[Arguments Optional](https://freecodecamp.cn/challenges/arguments-optional)