---
layout: post
title: "DNA Pairing(算法)"
keywords: ["FCC","算法","freecodecamp","DNA Pairing"]
description: "FCC算法题 DNA Pairing"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

DNA 链缺少配对的碱基。依据每一个碱基，为其找到配对的碱基，然后将结果作为第二个数组返回。

Base pairs（碱基对） 是一对 AT 和 CG，为给定的字母匹配缺失的碱基。

在每一个数组中将给定的字母作为第一个碱基返回。

例如，对于输入的 GCG，相应地返回 [["G", "C"], ["C","G"],["G", "C"]]

字母和与之配对的字母在一个数组内，然后所有数组再被组织起来封装进一个数组。

###提示

[Array.push()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/push)

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

###思路

数组的迭代方法`map()`会对数组的每一项运行给定函数，返回每次函数调用结果组成的数组。

###解法

<pre>
function pair(str) {
  var arr = str.split("");
  var pair = "";
  var result = arr.map(function(item,index,array){
    if(item === "A"){
      pair = "T";
    }else if(item === "T"){
      pair = "A";
    }else if(item === "C"){
      pair = "G";
    }else if(item === "G"){
      pair = "C";
    }
    return [item,pair];
  });
  return result;
}
</pre>

###测试

`pair("ATCGA")` 应该返回 [["A","T"],["T","A"],["C","G"],["G","C"],["A","T"]]。

`pair("TTGAG")` 应该返回 [["T","A"],["T","A"],["G","C"],["A","T"],["G","C"]]。

`pair("CTCTA")` 应该返回 [["C","G"],["T","A"],["C","G"],["T","A"],["A","T"]]。

###在线调试

[DNA Pairing](https://freecodecamp.cn/challenges/dna-pairing)