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
&lt;input type="button" value="clickVal" onclick="alert(this.value)"/>
</pre>

↑↑ 会弹出"clickVal"，这里的 this 指的是目标元素。

<pre>
//html文件
&lt;input type="button" value="clickVal" onclick="showEventType ()"/>

//javascript文件
function showEventType () {
    alert(event.type);	//"click"
}
</pre>

这样指定事件处理程序，会创建一个带有局部变量 event 的函数。通过 event，可以直接访问事件对象。

HTML事件处理程序有两个缺点。一是在 HTML 加载完成、对应的 js 文件未加载时点击按钮从而导致错误；二是不同浏览器差异下的作用域链问题；还有这样将导致 HTML 与 js 紧密耦合。

###DOM0级事件处理程序

将一个函数赋值给一个事件处理程序属性。

<pre>
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert(this.id);	//"myBtn"
};
</pre>

以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。

<pre>
//删除上一段代码定义的事件处理程序
btn.onclick = null;
</pre>

将事件处理程序设为 null 后，单击按钮也不会触发事件。

###DOM2级事件处理程序

DOM2级事件处理程序定义了两个方法：

`addEventListener(要处理的事件名,事件处理程序函数,布尔值)`

`removeEventListener(要处理的事件名,事件处理程序函数,布尔值)`

其中，参数的布尔值为 true，捕获阶段调用事件处理程序；为 false，冒泡阶段调用事件处理程序。（建议该值用 false）

<pre>
var btn = document.getElementById("ab3");
var showId = function(){
    alert(this.id);
}
btn.addEventListener("click",showId,false);
</pre>

<pre>
btn.removeEventListener("click",showId,false);
</pre>

通过`addEventListener()`添加的事件处理程序只能通过`removeEventListener()`来移除。也就是说，如果添加的第二个参数是匿名函数，将无法移除该事件处理程序。

###IE事件处理程序

有两个方法：

`attachEvent(事件处理程序名称,事件处理程序函数)`

`detachEvent(事件处理程序名称,事件处理程序函数)`



##测试

观看本文代码的演示效果，请移步[这里]