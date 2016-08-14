---
layout: post
title: "JavaScript中的事件"
keywords: ["JavaScript","事件","js","event","node"]
description: "介绍JavaScript中的事件"
category: "DOM"
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

以这种方式添加的事件处理程序会在事件流的冒泡阶段被处理。注意，对每个事件只支持一个事件处理程序。

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

可以为一个元素添加多个事件，事件发生顺序按代码先后顺序。

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

<pre>
var btn = document.getElementById("ab5");
var showThis = function(){
    alert(this === window);	//true
}
btn.attachEvent("onclick", showThis);
</pre>

`attachEvent()`中，事件处理的作用域为全局作用域。可以为一个元素添加多个事件，事件发生顺序与代码先后顺序相反。

<pre>
btn.detachEvent("onclick", showThis);
</pre>

###跨浏览器的事件处理程序

将方法封装在 EventUtil 对象里（该对象封装了处理浏览器间差异的通用方法）：

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

你可以这样调用：

<pre>
var btn6 = document.getElementById("ab6");
var sayHello = function(){
    alert("hello hello");
}
EventUtil.addHandler(btn6, "click", sayHello);
</pre>

##事件对象

触发 DOM 上某个事件时，会产生一个事件对象 event，该对象包含所有与事件有关的信息。所有浏览器都支持 event，但支持方式不同。

###跨浏览器的事件对象

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
    //返回对event对象的引用
    getEvent: function(event) {
        return event ? event : window.event;
    },
    //返回事件的目标
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    //取消事件的默认行为
    preventDefault: function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
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
    },
    //阻止事件冒泡
    stopPropagation: function(event) {
        if (event.stopPropagation) {
            event.stopPropagation();
        } else {
            event.cancelBubble = true;
        }
    }
};
</pre>

##测试

观看本文代码的演示效果，请移步[这里](http://blog.ilanyy.com/example/event/)。