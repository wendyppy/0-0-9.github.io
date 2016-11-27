---
layout: post
title: "玩坏前端"
keywords: ["前端 ","休闲"]
description: "前端休闲玩法"
category: "HTML5"
tags: ["HTML5"]
---
{% include JB/setup %}

发现自己好久没更博了，最近一直在努力学习数学，哈哈。十月的最后一天，分享一些有趣的东西。

## 浏览器To编辑器

在html5中新多了一个特性，叫做`contenteditable`,这个特性可以让html里的一个元素变成可编辑状态。因此，我们可以通过这个属性来构造一个简单的文本编辑器。

在浏览器里新建一个标签页输入网址`data:text/html, \<html contenteditable\>`你会发现你的浏览界面变成了一个简单的文本编辑器。

![image](http://cdn.saymagic.cn/141030232847.28.07.png)


往往有了简单的东西之后，就会有大神把它改造成神器，比如，用Chrome或者Safari打开下面的超长链接，

	data:text/html, <style type="text/css"># e{position:absolute;top:0;right:0;bottom:0;left:0;}</style><div id="e"></div><script src="http://d1n0x3qji82z53.cloudfront.net/src-min-noconflict/ace.js" type="text/javascript" charset="utf-8"></script><script>var e=ace.edit("e");e.setTheme("ace/theme/monokai");e.getSession().setMode("ace/mode/python");</script>
	

你会得到一个高亮python代码的编辑器，效果如下：
	
![image](http://cdn.saymagic.cn/141030233338.33.04.png)

如果想改造成支持其他语言语法高亮的，可把 上面链接里的ace/mode/python 替换为：
 
	C/C++ -> ace/mode/c_cpp
	Javscript -> ace/mode/javascript
	Java -> ace/mode/java
	Scala -> ace/mode/scala
	Markdown -> ace/mode/markdown
	and more...
	
##  修改网页为你想要的任何内容

有了上面的基础，我们可以做一点更有意思的东西，打开chrome的console控制台。输入`document.body.contentEditable='true';`

![image](http://cdn.saymagic.cn/141030234522.gif)

当console返回true后，说明你可以对当前的页面做任意的修改了。恶搞如下：

![image](http://cdn.saymagic.cn/141030234917.gif)

这比起以前需要在chrome的控制台里先点击小的搜索按钮锁定内容后再去修改简单很多了。

##  将网页进行反转

愚人节的时候有看到过将网页进行整体翻转的网站，实现起来也很简单，只需要在css文件里增加如下代码：

	html{
 	filter:fliph;
 	}/*ie fliph(水平翻转滤镜)，还有flipv垂直翻转滤镜*/
 	body{
 	-webkit-transform: rotateY(180deg);
 	transform: rotateY(180deg);
 	-moz-transform: skew(0deg, 180deg) scale(-1, 1);
 	-o-transform: skew(0deg, 180deg) scale(-1, 1);
 	}
 
 效果如下：
 
![image](http://cdn.saymagic.cn/141031000024.gif)

如果想让网页只旋转10度，则可以这样写：

	 body {
 	transform: rotate(-10deg);
	 -moz-transform: rotate(-10deg);
 	-webkit-transform: rotate(-10deg);
 	-o-transform: rotate(-10deg);
 	}

另外，还发现了一个叫做foo.js的东西，通过这个js文件我们可以构造很多愚人的页面特效。感兴趣可以关注这个github地址：[https://github.com/idiot/Fool.js](https://github.com/idiot/Fool.js)


