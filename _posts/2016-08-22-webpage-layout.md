---
layout: post
title: "css浮动布局"
keywords: ["css","网页布局","自适应","浮动"]
description: "利用CSS浮动特性进行网页布局"
category: "css"
tags: ["css","html"]
---
{% include JB/setup %}

本文中的自适应只考虑宽度，不包括高度。

##单列布局

给块级元素设置定宽，再设置`margin: 0 auto;`实现水平居中。需要注意的是，此时不能设置元素的 float 或绝对定位，否则居中失效。

举个🌰： 百度首页

<center>![](http://cdn.saymagic.cn/o_1arurutnf10uc19iq118814k01cq29.png)</center>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

##两列布局

###两列定宽

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">&lt;/div>
	&lt;div class="right">&lt;/div>
&lt;/div>
</pre>

<pre>
/* css样式 */
.wrap { width: 500px; margin: 0 auto; overflow: hidden; }
.left { float: left; width: 350px; }
.right { float: right; width: 148px; }
</pre>

这里面，给容器（父元素）设置`overflow: hidden`可避免父元素没被撑开，高度塌陷（这样闭合浮动代码量很少，但内容增多时会有溢出内容被隐藏的问题）。

举个🌰： 腾讯网

<center>![](http://cdn.saymagic.cn/o_1arus6qqu2tcqe7f706pl1d15e.png)</center>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

###两列自适应

把两栏包裹在一个容器里，设置两栏宽度时用和为 100% 表示就 OK 了。

###左侧定宽，右侧自适应

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">&lt;/div>
	&lt;div class="right">
		&lt;div class="right-child">&lt;/div>
	&lt;/div>
&lt;/div>
</pre>

一般来说，定宽的较窄，用作侧边栏，而自适应宽度的往往是主体部分。本例中，有一个定宽的左侧边栏和一个自适应宽度的右边主体部分。主要利用的是 float 属性和 margin 的负值来实现这样的效果。

首先，左、右栏被包裹在一个容器里面。

给左侧边栏设置向左浮动以及一个较小的宽度，它的 margin-right 为其宽度的负值。例如，`width: 100px; margin-right: -100px;`。这样是为了让文档流中的下一个元素即右栏内容填充这部分空间。

右侧栏设置向右浮动，宽度充满父容器，即设为 100%。

右侧栏的内容、背景等要设给它的子元素，例中是`right-child`。对`right-child`，设置其 margin-left 为左侧边栏宽度加上左右栏之间的空隙宽度。

<pre>
/* css样式 */
.wrap { overflow: hidden; }
.left { float: left; width: 190px; margin-right: -190px; }
.right { float: right; width: 100%; }
.right-child { margin-left: 200px; }
</pre>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

###左侧自适应，右侧定宽

这种效果的实现与上面的左侧定宽，右侧自适应道理相同。

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">
		&lt;div class="left-child">&lt;/div>
	&lt;/div>
	&lt;div class="right">&lt;/div>
&lt;/div>
</pre>

<pre>
/* css样式 */
.wrap { overflow: hidden; }
.left { float: left; width: 100%; }
.left-child { margin-right: 200px; }
.right { float: right; width: 190px; margin-left: -190px;}
</pre>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

##三列布局

###左右定宽，中间自适应

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">&lt;/div>
	&lt;div class="mid">
		&lt;div class="mid-child">&lt;/div>
	&lt;/div>
	&lt;div class="right">&lt;/div>
&lt;/div>
</pre>

<pre>
/* css样式 */
.wrap { overflow: hidden; }
.left { float: left; width: 100px; margin: 0 -100px 0 0; }
.right { float: right; width: 190px; margin: 0 0 0 -190px; }
.mid { float: left; width: 100%; }
.mid-child { margin: 0 200px 0 110px; }
</pre>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

###中右定宽，左边自适应

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">
		&lt;div class="left-child">&lt;/div>
	&lt;/div>
	&lt;div class="right">&lt;/div>
	&lt;div class="mid">&lt;/div>
&lt;/div>
</pre>

<pre>
/* css样式 */
.wrap { overflow: hidden; }
.left { float: left; margin-right: -290px; width: 100%; }
.left-child { margin-right: 300px; }
.mid { float: right; width: 190px; }
.right { float: right; width: 90px; margin-left: 10px; }    
</pre>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

###中左定宽，右边自适应

<pre>
&lt;!-- html结构 -->
&lt;div class="wrap">
	&lt;div class="left">&lt;/div>
	&lt;div class="mid">&lt;/div>
	&lt;div class="right">
		&lt;div class="right-child">&lt;/div>
	&lt;/div>
&lt;/div>
</pre>

<pre>
/* css样式 */
.wrap { overflow: hidden; }
.left { float: left; margin-right: 10px; width: 190px; }
.mid { float: left; width: 90px; }
.right { float: right; width: 100%; margin-left: -290px; }
.right-child { margin-left: 300px; }
</pre>

效果： [所有演示均在此地址](http://blog.ilanyy.com/example/csslayout/)

##闭合浮动

摘录了网上比较流行的一种做法：

<pre>
.clearfix:after {
    display: block;
    clear: both;
    height: 0;
    visibility: hidden;
    content: '.';
}
.clearfix {
    zoom: 1
}
</pre>

在本文代码中，给 wrap 层追加 clearfix 类也能解决子元素浮动带来的影响。