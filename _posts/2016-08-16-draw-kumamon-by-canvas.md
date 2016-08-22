---
layout: post
title: "手把手教你用canvas画熊本熊"
keywords: ["canvas","html5","绘图","熊本熊"]
description: "使用 html5 canvas 画熊本熊"
category: "HTML5"
tags: ["HTML5","JavaScript"]
---
{% include JB/setup %}

canvas 是 H5 一个超棒的元素，能创造好多漂亮的效果。现在就用它最简单的几个方法，画一只酷炫的熊本熊（恩，酷炫是因为熊本熊本来就是一种酷炫的生物）。

##准备工作

支持 canvas 的现代浏览器，html 页面、css 文件、js 文件。

##HTML & CSS

先写结构和样式：

<pre>
&lt;!-- html文件 -->
&lt;canvas class="kumamon" id="kumamon" width="500" height="500">kumamon</canvas>
</pre>

为了清楚地看到画布位置，我们来给画布加个黑色边框：
<pre>
/\*css文件*/
.kumamon{
    border: 1px solid #000;
}
</pre>

##JS

JavaScript 脚本是绘制熊本的重要部分。在动手前，我们先找一张成品图进行模仿：

<center><div class="smPic">![](http://cdn.saymagic.cn/o_1aqojfiqd14tv1qqacsf1u2emcc9.jpg)</div></center>

这个大体能分成三部分：**背景、圈里的熊、文字**。

###框架

画整幅图的函数：

<pre>
function drawKumamon() {
    var ku = document.getElementById("kumamon");
    if (ku.getContext) {
        var ctx = ku.getContext("2d");
        //绘图代码
    }
}
</pre>

文档加载后绘图（防止`getElementById()`取到空值）：

<pre>
window.onload = function() {
    drawKumamon();
}
</pre>

跨浏览器的事件监听方法（本例中只会用到 addHandler）：

<pre>
var EventUtil = {
    addHandler: function(ele, type, handler) {
        if (ele.addEventListener) {
            ele.addEventListener(type, handler, false);
        } else if (ele.attachEvent) {
            ele.attachEvent("on" + type, handler);
        } else {
            ele["on" + type] = handler;
        }
    },
    removeHandler: function(ele, type, handler) {
        if (ele.removeEventListener) {
            ele.removeEventListener(type, handler, false);
        } else if (ele.detachEvent) {
            ele.detachEvent("on" + type, handler);
        } else {
            ele["on" + type] = null;
        }
    }
};
</pre>

###背景

为了让接下来的白色圈等效果更加明显，我们先涂背景。这里背景用纯色，暂定为深红色，颜色值为`rgb(174,0,0)`。

我们把制作背景色的代码封装成一个函数，然后在主函数中调用。这类工具绘图类函数我们以 paintXXX 命名。

<pre>
function paintBackground(ctx, sColor) {
    v_color = sColor;
    ctx.fillStyle = sColor;
    ctx.fillRect(0, 0, 500, 500);
}
var v_color = "rgb(174,0,0)";
</pre>

在主函数`drawKumamon()`中调用：

<pre>
paintBackground(ctx, v_color);
</pre>

###圆圈

熊本住在一个白色的圆圈里，它脸上的腮红也是圆圈。所以同样的，我们把画圆的代码封装成一个函数。

<pre>
function paintCircle(ctx, x, y, r, i, sColor) {
    ctx.beginPath();
    ctx.moveTo(x - r, y);
    ctx.arc(x, y, r, 0, i * Math.PI, false);
    ctx.fillStyle = sColor;
    ctx.fill();
}
</pre>

在主函数中调用：

<pre>
paintCircle(ctx, 250, 224, 191, 2, "#fff");
</pre>

这样，就在画布中间偏上的位置上有一个住着熊本熊的圆了。

目前效果：

<center><div class="smPic">![](http://cdn.saymagic.cn/o_1aqpbbkh51pcaqpk1cvd34sk5ie.png)</div></center>

###熊耳朵

因为 canvas 默认的遮盖效果是`source-over`，即新画的源图像遮挡原先画的目标图像。观察熊本，发现它的脸在耳朵上面，五官在脸上面，头在身体上面。所以如果把熊本拆成这几块的话，绘图顺序应该是**身体 → 耳朵 → 脸 → 五官** 。 