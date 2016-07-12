---
layout: post
title: "Friendly Date Ranges(算法)"
keywords: ["FCC ","算法"]
description: "FCC算法题"
category: "FCC"
tags: ["FCC","高级算法","algorithm","Date","String"]
---
{% include JB/setup %}

###题目

把个个格式化的日期区间如两个 YYYY-MM-DD 转换成一种更易读的格式.

易读格式应该是用月份名称代替月份数字，用序数词代替数字来表示天 (1st 代替 1).

不要显示那些可以被推测出来的信息: 如果一个日期区间里结束日期与开始日期相差不超一年，则结束日期就不用写年份了. 月份开始和结束日期如果在同一个月，则结束日期月份就不用写了.

另外, 如果日期区间内开始日期年份是当前年份，且结束日期与开始不超过一年，则开始日期的年份也不用写.

例如:

`makeFriendlyDates(["2016-07-01", "2016-07-04"])` 应该返回 `["July 1st","4th"]`

`makeFriendlyDates(["2016-07-01", "2018-07-04"])` 应该返回 `["July 1st, 2016", "July 4th, 2018"]`.

###提示

[String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

[String.substr()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr)

[parseInt()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

###思路

先考虑主要逻辑，这题要我们按要求格式化传入日期。按题意要求，我们来分步解答。

取得字符串形式的开始日期、结束日期，存入变量`temp`。

<pre>
var temp = function(i){
    return arr[i].split("-");
  };
</pre>

`temp(i)`（ i 的值为0、1，分别代表开始、结束日期）打印出来的格式是这样：

<pre>
["2022", "09", "05"]
</pre>

“用月份名称代替月份数字”。新建数组 `months` ，准备与取得的日期建立对应关系。

<pre>
var months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
</pre>

建立输出格式的代表月份的函数`m()`：

<pre>
var m = function(month){
    return months[parseInt(temp(month)[1])-1];
  };
</pre>

数组的索引是从零开始的，输入的月份是从一开始的，为了保持对应，记得要减一哦。

“用序数词代替数字来表示天 (1st 代替 1)”。

<pre>
var d = function(day){
    if(day == "01"){
      return "1st";
    }else if(day == "02"){
      return "2nd";
    }else if(day == "03"){
      return "3rd";
    }else if(day >= 4){
      return Number(day) + "th";
    }
  };
</pre>

这段代码中要注意的是在判断`day >= 4`的地方，因为比较运算符可以自动进行类型转换，所以这个比较是没问题的。返回值如果不用`Number()`进行类型转换的话，转换后个位数的日期会像这样 <code class="txt">04th</code>.而`Number()`转换类型时会忽略前导零，将返回我们期望的正确值 <code class="txt">4th</code>。

用以上三个变量我们可以拼接出正常情况的返回值。

<pre>
var output = function(i){
    return m(i) + " " + d(temp(i)[2]) + "," + " " + temp(i)[0];
  };
</pre>

判断逻辑开始之前，先做点前期准备：

<pre>
var result = [];			//将返回的数组，最后应包含开始、结束日期
var str0 = output(0);	//开始日期
var str1 = output(1);	//结束日期
</pre>

比较两个日期相差天数可以这样表示：

<pre>
var intervalDays = parseInt(Math.abs(new Date(temp(0)) - new Date(temp(1)))/1000/60/60/24) + 1;
</pre>

好了，让我们再看看额外要求。

“如果一个日期区间里结束日期与开始日期相差不超一年，则结束日期就不用写年份了”。

<pre>
if(intervalDays <= 365){
    str1 = str1.slice(0,output(1).indexOf(","));
  }
</pre>

“月份开始和结束日期如果在同一个月，则结束日期月份就不用写了”。

<pre>
if(intervalDays < 32 && m(0) === m(1)){
    str1 = str1.slice(output(1).indexOf(" ") + 1);
  }
</pre>

“如果日期区间内开始日期年份是当前年份，且结束日期与开始不超过一年，则开始日期的年份也不用写”。

<pre>
if(temp(0)[0] == now.getFullYear() && intervalDays < 365){
    str0 = str0.slice(0,output(0).indexOf(","));
    str1 = str1.slice(0,output(1).indexOf(","));
  }
</pre>

另外要考虑的一种情况是，如果开始日期和结束日期是同一天的话，只要返回一个日期就好了。

<pre>
if(temp(0).toString() === temp(1).toString()){
    result.push(str0);
    return result;
  }
</pre>

除此之外的情况：

<pre>
result.push(str0,str1);
</pre>

最后函数返回`result`，顺利解决！

###解法

<pre>
function makeFriendlyDates(arr) {
  var months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
  var now = new Date();
  var result = [];
  var temp = function(i){
    return arr[i].split("-");
  };
  var m = function(month){
    return months[parseInt(temp(month)[1])-1];
  };
  var d = function(day){
    if(day == "01"){
      return "1st";
    }else if(day == "02"){
      return "2nd";
    }else if(day == "03"){
      return "3rd";
    }else if(day >= 4){
      return Number(day) + "th";
    }
  };
  var intervalDays = parseInt(Math.abs(new Date(temp(0)) - new Date(temp(1)))/1000/60/60/24) + 1;
  var output = function(i){
    return m(i) + " " + d(temp(i)[2]) + "," + " " + temp(i)[0];
  };
  var str0 = output(0);
  var str1 = output(1);
  if(intervalDays <= 365){
    str1 = str1.slice(0,output(1).indexOf(","));
  }
  if(intervalDays < 32 && m(0) === m(1)){
    str1 = str1.slice(output(1).indexOf(" ") + 1);
  }
  if(temp(0)[0] == now.getFullYear() && intervalDays < 365){
    str0 = str0.slice(0,output(0).indexOf(","));
    str1 = str1.slice(0,output(1).indexOf(","));
  }
  if(temp(0).toString() === temp(1).toString()){
    result.push(str0);
    return result;
  }
  result.push(str0,str1);
  return result;
}    
</pre>

###测试

`makeFriendlyDates(["2016-07-01", "2016-07-04"])` should return ["July 1st","4th"].

`makeFriendlyDates(["2016-12-01", "2017-02-03"])` should return ["December 1st","February 3rd"].

`makeFriendlyDates(["2016-12-01", "2018-02-03"])` should return ["December 1st, 2016","February 3rd, 2018"].

`makeFriendlyDates(["2017-03-01", "2017-05-05"])` should return ["March 1st, 2017","May 5th"]

`makeFriendlyDates(["2018-01-13", "2018-01-13"])` should return ["January 13th, 2018"].

`makeFriendlyDates(["2022-09-05", "2023-09-04"])` should return ["September 5th, 2022","September 4th"].

`makeFriendlyDates(["2022-09-05", "2023-09-05"])` should return ["September 5th, 2022","September 5th, 2023"].
