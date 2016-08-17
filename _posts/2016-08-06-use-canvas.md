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