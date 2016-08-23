---
layout: post
title: "canvas 绘图"
keywords: ["canvas","html5","绘图","JavaScript"]
description: "使用 html5 canvas 进行绘图"
category: "HTML5"
tags: ["HTML5","JavaScript"]
---
{% include JB/setup %}

最近了解了一些 canvas 绘图相关知识，现整理出来。

##准备知识

作为一个 HTML 元素，`<canvas>`在页面中设定一个区域，然后用 JavaScript 在这个区域中绘制图形。注意，`<canvas>`只是一个容器，JavaScript 才做着绘图的工作。你可以用它来制作照片集或动画。

想用它创造一些很棒的效果，我们需要相关的 HTML、JavaScript 知识和支持该元素的新版本浏览器。
<center>
![IE9+](http://cdn.saymagic.cn/o_1aqcdo2cldv1pt8rfj1m88d3jj.gif)
![FireFox 1.5+](http://cdn.saymagic.cn/o_1aqcdnshkp0e3901dt11k0u95ve.gif)
![Opera 9+](http://cdn.saymagic.cn/o_1aqcdu4pv11rc1481n9m1cth9gs12.gif)
![Safari 2+](http://cdn.saymagic.cn/o_1aqcdq1o31pge17i11a6r19cj1c3lt.gif)
![Chrome](http://cdn.saymagic.cn/o_1aqcdnnnc1kif88ke3810reb29.gif)
<br>
<caption>IE9+，FireFox 1.5+，Opera 9+，Safari 2+，Chrome</caption>
</center>

##基本用法

<pre>
&lt;canvas id="myCanvas" width="300" height="250">替代内容&lt;/canvas>
</pre>

`<canvas>`只有两个属性：**width** 和 **height**。不明确指定宽高时，默认宽 300px，高 150px。

###替代内容

在不支持`<canvas>`的浏览器中，你可以放入文字或图片作为替代内容显示。当然啦，在支持`<canvas>`的浏览器中，替代内容将不会显示。

替代内容示例：

<pre>
&lt;canvas id="myCanvas" width="300" height="250">这里是替代内容！Selina大美女~&lt;/canvas>

&lt;canvas id="myCanvas" width="300" height="250">
&lt;img src="http://cdn.saymagic.cn/o_1aqcfmcjj1snvep8qlg18g7t2q9.jpg" width="300" height="250">
&lt;/canvas>
</pre>

###渲染上下文

canvas 开始是空白的。要先找到渲染上下文，才能开始绘图。`<canvas>`的`getContext(上下文格式)`方法可以获得渲染上下文以及其绘画功能。

<pre>
var canvas = document.getElementById("myCanvas");
//确定浏览器支持 canvas
if (canvas.getContext){
	var context = canvas.getContext("2d");
	//绘制代码
}
</pre>

##绘制图形

2D 上下文的坐标以`<canvas>`画布的左上角为起点：

![](http://cdn.saymagic.cn/o_1aqcjjg5s6gn1oqu1n2ktlb1kdm9.jpg)

###绘制矩形（Rectangular）

<pre>
fillRect(x, y, width, height)	//绘制一个填充的矩形

strokeRect(x, y, width, height)	//绘制矩形边框

clearRect(x, y, width, height)	//清除指定矩形区域，让清除部分完全透明
</pre>

描边（`strokeRect()`）、填充（`fillRect()`）默认为黑色，也可用颜色名、十六进制码、rgb、rgba、hsl、hsla指定颜色。

如：

<pre>
function drawMyCanvas(){
    var canvas = document.getElementById("myCanvas");
    if (canvas.getContext) {
        var context = canvas.getContext("2d");
        //绘制矩形
        context.fillStyle = "#c00"
        context.fillRect(60, 50, 150, 125);
        context.strokeStyle = "yellow";
        context.strokeRect(70, 60, 130, 105);
        context.clearRect(90, 80, 90, 65);
    };
}
</pre>

##绘制路径

###开始绘制

`beginPath()`
表示开始一条新路径。绘制路径前需调用该方法。

###进行绘制

`arc(x, y, radius, startAngle, endAngle, counterclockwise)`
以 (x,y) 为圆心绘制一条弧线，弧线半径为 radius，弧度表示的起始、结束角度分别为 startAngle、endAngle；counterclockwise 值为 false（默认）表示顺时针方向计算。

`arcTo(x1, y1, x2, y2, radius)`
从上一点开始到 (x2,y2) 绘制圆弧，给定的半径 radius 穿过 (x1,y1)。

P.S. arc()、arcTo()中角度单位为弧度。

<pre>
//JavaScript弧度、角度换算公式
radians = (Math.PI / 180) * degrees
</pre>



##测试

观看本文中具体代码效果，请移步[这里](http://blog.ilanyy.com/example/canvas/)。