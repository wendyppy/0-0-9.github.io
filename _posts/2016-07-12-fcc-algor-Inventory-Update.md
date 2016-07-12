---
layout: post
title: "Inventory Update(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","高级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

依照一个存着新进货物的二维数组，更新存着现有库存(在 arr1 中)的二维数组. 如果货物已存在则更新数量 . 如果没有对应货物则把其加入到数组中，更新最新的数量. 返回当前的库存数组，且按货物名称的字母顺序排列.

###提示

[Global Array Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)

###思路

分析题意，现有库存`arr1`和新进货物`arr2`都是二维数组，每一项又能拆分为`value`、`key`两种。我们要把`arr2`中的`key`与`arr1`中的`key`进行比较，如果相同，则把`arr1`中相应的`value`值加上新进货物进行更新，如果`arr1`中的`key`没有相同值，说明该项是新品种货物，需要把整项加入`arr1`数组进行更新。

当现有库存`arr1`为空时，新进货物将全部更新为库存：

<pre>
if(arr1.length === 0){
    arr1 = arr2;
  }
</pre>

将`arr1`中每一项与`arr2`逐条对比：

<pre>
arr1.forEach(function(item,index,array){
      for(var i = 0; i < arr2.length; i++){
         //do something...      }
    });
</pre>

当`arr1`中已存在相同货物时：

<pre>
if(item[1] == arr2[i][1]){
            item[0] += arr2[i][0];
            break;
          }
</pre>

`arr1`中尚未有该货物：

<pre>
if(array.join().indexOf(arr2[i][1]) < 0){
            array.push(arr2[i]);
          }
</pre>

循环外部对得到的`arr1`数组按`key`进行升序排序：

<pre>
arr1.sort(function(a,b){
    return a[1] > b[1];
  });
</pre>

###解法

<pre>
function updateInventory(arr1, arr2) {
  // All inventory must be accounted for or you're fired!
  if(arr1.length === 0){
    arr1 = arr2;
  }
  else{
    arr1.forEach(function(item,index,array){
      for(var i = 0; i < arr2.length; i++){
        //货物已存在
        if(item[1] == arr2[i][1]){
           item[0] += arr2[i][0];
           break;
        }
        //无对应货物
        if(array.join().indexOf(arr2[i][1]) < 0){
           array.push(arr2[i]);
        }    
      }
    });
  }
  arr1.sort(function(a,b){
    return a[1] > b[1];
  });
  return arr1;
}
</pre>

###测试

<code class="txt">updateInventory()</code>应该返回一个数组.

<code class="txt">updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]]).length</code> 应该返回一个长度为6的数组.

<code class="txt">updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]])</code> 应该返回 <code class="txt">[[88, "Bowling Ball"], [2, "Dirty Sock"], [3, "Hair Pin"], [3, "Half-Eaten Apple"], [5, "Microphone"], [7, "Toothpaste"]]</code>.

<code class="txt">updateInventory([[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]], [])</code> 应该返回 <code class="txt">[[21, "Bowling Ball"], [2, "Dirty Sock"], [1, "Hair Pin"], [5, "Microphone"]]</code>.

<code class="txt">updateInventory([], [[2, "Hair Pin"], [3, "Half-Eaten Apple"], [67, "Bowling Ball"], [7, "Toothpaste"]])</code> 应该返回 <code class="txt">[[67, "Bowling Ball"], [2, "Hair Pin"], [3, "Half-Eaten Apple"], [7, "Toothpaste"]]</code>.

<code class="txt">updateInventory([[0, "Bowling Ball"], [0, "Dirty Sock"], [0, "Hair Pin"], [0, "Microphone"]], [[1, "Hair Pin"], [1, "Half-Eaten Apple"], [1, "Bowling Ball"], [1, "Toothpaste"]])</code> 应该返回 <code class="txt">[[1, "Bowling Ball"], [0, "Dirty Sock"], [1, "Hair Pin"], [1, "Half-Eaten Apple"], [0, "Microphone"], [1, "Toothpaste"]]</code>.