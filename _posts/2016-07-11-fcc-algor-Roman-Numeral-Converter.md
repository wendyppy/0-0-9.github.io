---
layout: post
title: "Roman Numeral Converter(算法)"
keywords: ["FCC ","算法","freecodecamp","Roman Numeral Converter"]
description: "FCC算法题 Roman Numeral Converter"
category: "FCC"
tags: ["FCC","中级算法","algorithm","Array"]
---
{% include JB/setup %}

###题目

将给定的数字转换成罗马数字。

所有返回的 罗马数字 都应该是大写形式。

###提示

[Roman Numerals](http://www.mathsisfun.com/roman-numerals.html)

[Array.splice()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)

[Array.indexOf()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

[Array.join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

###思路

将给定数字转换成罗马数字，为了便于计算，大体思路是把传进来的数值和数组联系起来，经过系列操作后，最后转换成字符串。

单个罗马数字格式有一些规则：

1. 当一个罗马数字符号出现在比它更大的数字的右边时，要把它们加起来得到最终结果数字。

	例如：`VI = V + I = 5 + 1 = 6`
	
2. 当一个罗马数字符号出现在比它更大的数字的左边时，要把它们相减得到最终结果数字。

	例如：`IX = X - I = 10 - 1 = 9`
	
3. 同一个罗马数字内部不能包含3个以上相同字符。

	例如：`IIII`是非法的，表示4应用`IV`。
	
摘录了一些代码中可能用到的罗马数字与阿拉伯数字对应关系，如下表所示：

<table>
<tr>
	<td>I</td>
	<td>IV</td>
	<td>V</td>
	<td>IX</td>
	<td>X</td>
	<td>XL</td>
	<td>L</td>
	<td>XC</td>
	<td>C</td>
	<td>CD</td>
	<td>D</td>
	<td>CM</td>
	<td>M</td>
</tr>
<tr>
	<td>1</td>
	<td>4</td>
	<td>5</td>
	<td>9</td>
	<td>10</td>
	<td>40</td>
	<td>50</td>
	<td>90</td>
	<td>100</td>
	<td>400</td>
	<td>500</td>
	<td>900</td>
	<td>1000</td>
</tr>
</table>

观察上表可以想到，创建两个数组，分别放入表中每行内容，依靠其对应关系进行转化。

<pre>
var nums = [1000,900,500,400,100,90,50,40,10,9,5,4,1];
var romans =["m","cm","d","cd","c","xc","l","xl","x","ix","v","iv","i"];
</pre>

再看一下资料给定的罗马数字拼接规则：

>Break the number into Thousands, Hundreds, Tens and Ones, and write down each in turn.

翻译过来是，把给定数字拆分为千位、百位、十位、个位，然后再由大至小依次进行拼接。

5000以上的罗马数字比较特殊，很少用到，本题中不予讨论。

因为多位罗马数字是高位到低位，因此我们创建的两个数组最好也是由大到小，先讨论高位，再讨论低位。

声明一个空字符串，以便写入罗马字符串的拼接。

<pre>
var str = '';
</pre>

对 nums 的每一项进行遍历，每当传入的 num 值大于或等于当前项时，将其对应的罗马数字推入之前声明的字符串，并把 num 值减去当前符合条件的数值。

<pre>
nums.forEach(function(item,index,array){
    while(num >= item){
      str += romans[index];
      num -= item;
    }
  });
</pre>

最后不要忘记把返回结果转为大写哦！

###解法

<pre>
function convert(num) {
  var nums = [1000,900,500,400,100,90,50,40,10,9,5,4,1];
  var romans =["m","cm","d","cd","c","xc","l","xl","x","ix","v","iv","i"];
  var str = '';
  nums.forEach(function(item,index,array){
    while(num >= item){
      str += romans[index];
      num -= item;
    }
  });
  
 return str.toUpperCase();
}
</pre>

###测试

convert(12) 应该返回 "XII"。

convert(16) 应该返回 "XVI"。

convert(29) 应该返回 "XXIX"。

convert(44) 应该返回 "XLIV"。

convert(45) 应该返回 "XLV"。

convert(68) 应该返回 "LXVIII"。

convert(83) 应该返回 "LXXXIII"。

convert(97) 应该返回 "XCVII"。

convert(99) 应该返回 "XCIX"。

convert(500) 应该返回 "D"。

convert(501) 应该返回 "DI"。

convert(649) 应该返回 "DCXLIX"。

convert(798) 应该返回 "DCCXCVIII"。

convert(891) 应该返回 "DCCCXCI"。

convert(1000) 应该返回 "M"。

convert(1004) 应该返回 "MIV"。

convert(1006) 应该返回 "MVI"。

convert(1023) 应该返回 "MXXIII"。

convert(2014) 应该返回 "MMXIV"。

convert(3999) 应该返回 "MMMCMXCIX"。