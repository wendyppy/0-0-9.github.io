---
layout: post
title: "JavaScript中的事件"
keywords: ["JavaScript","事件","js","event","node"]
description: "介绍JavaScript中的事件"
category: "JavaScript"
tags: ["JavaScript","DOM"]
---
{% include JB/setup %}

##DOM事件流

DOM2 级事件规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。

举个 🌰，在一个标准的 HTML 页面中点击某个`<div>`元素，触发顺序如下：

![](http://cdn.saymagic.cn/o_1aq1ehll1r6t1ejsnoh10a51de09.jpeg)

DOM 事件流中，`<div>`在捕获阶段不会接收到事件。在处于目标阶段，事件在`<div>`上发生，在事件处理阶段被看成是冒泡阶段的一部分。冒泡阶段，事件又传回文档。

##事件处理程序

用户或浏览器执行的动作是事件，如 click，load，mouseover。

事件处理程序是响应事件的函数，以 "on" 开头，如 onclick，onload，onmouseover。
 
###HTML事件处理程序

用与相应事件处理程序同名的 HTML 特性来指定事件。

<pre>
<input type="button" value="aClick" onclick="showId()">
</pre>

##测试

观看本文代码的演示效果，请移步[这里]