---
layout: post
title: "Where art thou(算法)"
keywords: ["FCC","算法","freecodecamp","Where art thou"]
description: "FCC算法题 Where art thou"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Object"]
---
{% include JB/setup %}

###题目

写一个 function，它遍历一个对象数组（第一个参数）并返回一个包含相匹配的属性-值对（第二个参数）的所有对象的数组。如果返回的数组中包含 source 对象的属性-值对，那么此对象的每一个属性-值对都必须存在于 collection 的对象中。

例如，如果第一个参数是 `[{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }]`，第二个参数是 `{ last: "Capulet" }`，那么你必须从数组（第一个参数）返回其中的第三个对象，因为它包含了作为第二个参数传递的属性-值对。

###提示

[Global Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

[Object.hasOwnProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)

[Object.keys()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

###思路

如果把一个对象表示为 `{key1: value1,key2: value2,...,keyN: valueN}` 形式，那么`Object.keys()`就能拿到`[key1,key2,...,keyN]`的集合。来看下它的定义：

`Object.keys()` 方法会返回一个由给定对象的所有可枚举自身属性的属性名组成的数组，数组中属性名的排列顺序和使用for-in循环遍历该对象时返回的顺序一致。

我们先拿到 source 的 key 值，然后再一一与 collection 中的每一项比较。

	var keys = Object.keys(source); 
	var arr = collection.filter(function(item){
		//do something...
	});

我们知道，`filter()`作为数组的迭代方法，会对数组的每一项运行给定函数，最后返回能使该函数返回 true 的项组成的数组。

**解法一**

设置循环，遍历 source 的 key 值所组成的数组 keys。

	for(var i = 0; i < keys.length; i++){
      //do something... 
    }   

在二重循环（filter 底层也是由循环实现的）的内部，做出如下判断：

	if(!item.hasOwnProperty(keys[i]) || item[keys[i]] !== source[keys[i]]){
        return false;
      }   

举个🌰 。有待测试函数
	`where([{ "a": 1, "b": 2 }, { "a": 1 }, { "a": 1, "b": 2, "c": 2 }], { "a": 1, "b": 2 })`。

易知 
	`collection = [{ "a": 1, "b": 2 }, { "a": 1 }, { "a": 1, "b": 2, "c": 2 }]`，
	`source = { "a": 1, "b": 2 }`，`keys = ["a","b"]`。

<code class="txt">obj.hasOwnProperty(prop)</code>在属性 prop 存在于对象实例时返回 true。

解释这个if判断语句：当 collection 的某一项（本例中可以是`{ "a": 1, "b": 2 }`、`{ "a": 1 }`或`{ "a": 1, "b": 2, "c": 2 }`）不存在属性 keys[i]（本例中可能是a 或 b）时，返回 false；item 在 key 值为 keys[i] 时 value 值不等于同 key 值时 source 的 value值，返回 false。

其余情况，在循环外部返回 true。P.S. 像这里 keys 可能包含多个项，这种情况下用排除法删除不符合要求的项比返回符合要求的项要简单许多。

**解法二**

用 `every()` 代替循环。这种方法虽然写起来简单得多，但效率上是比不上原始的 for 循环的。可以用谷歌的 `console.time()` 自己测一下函数的运行时间。

###解法

**解法一**

<pre>
function where(collection, source) {
  var arr = [];
  // What's in a name?
  var keys = Object.keys(source);
  arr = collection.filter(function(item){
    for(var i = 0; i < keys.length; i++){
      if(!item.hasOwnProperty(keys[i]) || item[keys[i]] !== source[keys[i]]){
        return false;
      }    
    }   
    return true;
  });
  return arr;
}
</pre>

**解法二**

<pre>
function where(collection, source) {
  var arr = [];
  // What's in a name?
  var keys = Object.keys(source);
  arr = collection.filter(function(item) {
    return keys.every(function(key) {
      return item.hasOwnProperty(key) && item[key] === source[key];
    });
  }); 
  return arr;
}
</pre>

###测试

`where([{ first: "Romeo", last: "Montague" }, { first: "Mercutio", last: null }, { first: "Tybalt", last: "Capulet" }], { last: "Capulet" })` 应该返回 [{ first: "Tybalt", last: "Capulet" }]。

`where([{ "a": 1 }, { "a": 1 }, { "a": 1, "b": 2 }], { "a": 1 })` 应该返回 [{ "a": 1 }, { "a": 1 }, { "a": 1, "b": 2 }]。

`where([{ "a": 1, "b": 2 }, { "a": 1 }, { "a": 1, "b": 2, "c": 2 }], { "a": 1, "b": 2 })` 应该返回 [{ "a": 1, "b": 2 }, { "a": 1, "b": 2, "c": 2 }]。

`where([{ "a": 1, "b": 2 }, { "a": 1 }, { "a": 1, "b": 2, "c": 2 }], { "a": 1, "c": 2 })` 应该返回 [{ "a": 1, "b": 2, "c": 2 }]。

###在线调试

[Where art thou](https://freecodecamp.cn/challenges/where-art-thou)