---
layout: post
title: "DOM中的节点类型"
keywords: ["JavaScript","DOM","js","节点类型","node"]
description: "详细介绍DOM中的节点类型"
category: "HTML"
tags: ["HTML","DOM","XML"]
---
{% include JB/setup %}

##概览

DOM（文档对象模型）是针对 HTML 和 XML 文档的一个 API。描绘了一个层次化的节点树，允许开发人员添加、移除、修改页面的某一部分。

举个 🌰：

<pre>
&lt;html&gt;
	&lt;head&gt;
		&lt;title&gt;DOM中的节点类型&lt;/title&gt;
	&lt;/head&gt;
	&lt;body&gt;
		&lt;p&gt;DOM是...&lt;/p&gt;
	&lt;/body&gt;
&lt;/html&gt;
</pre>

这段代码的层次结构表现为：

![](http://cdn.saymagic.cn/o_1apsfda5311ce4v8jkt1c081ohv9.jpg)

JavaScript 所有节点类型都继承自 Node 类型，该类型在除 IE 外的所有浏览器都可以访问到，所有节点类型都共享相同的基本属性和方法。

###节点的属性

节点的 nodeType 属性用于表明节点类型，对应12个数值常量。↓↓↓序号即为对应数值常量。

1. <code class="txt">Node.ELEMENT_NODE</code>
2. `Node.ATTRIBUTE_NODE`
3. <code class="txt">Node.TEXT_NODE</code>
4. `CDATA_SECTION_NODE`
5. `Node.ENTITY_REFERENCE_NODE`
6. `Node.ENTITY_NODE`
7. `Node.PROCESSING_INSTRUCTION_NODE`
8. `Node.COMMENT_NODE`
9. <code class="txt">Node.DOCUMENT_NODE</code>
10. `Node.DOCUMENT_TYPE_NODE`
11. `Node.DOCUMENT_FRAGMENT_NODE`
12. `Node.NOTATION_NODE`

查看`<body>`节点是否是一个元素：

<pre>
//IE下无效
var body = document.body;
if(body.nodeType == Node.ELEMENT_NODE){
    alert("Element!");
}
</pre>

因为 IE 没有公开 Node 类型的构造函数，上面的代码在 IE 中不能正常运行。我们可以用节点类型对应的数值常量来重写，使其跨浏览器兼容：

<pre>
var body = document.body;
if(body.nodeType === 1){
    alert("Element!");
}
</pre>

确定 nodeType 后，可以使用 nodeName 和 nodeValue 来了解节点的更多信息。

<pre>
var body = document.body;
var name,val;
if(body.nodeType === 1){
    name = body.nodeName;
    val = body.nodeValue;
    alert("nodeName:" + name + " nodeValue:" + val);	//"nodeName: BODY nodeValue:null"
}
</pre>

对于元素节点，nodeName 是标签名， nodeValue 始终为 null。

ownerDocument 也是所有节点都有的一个属性，指向表示整个文档的文档节点。

###节点关系

节点关系类似家族关系。

**子节点：** `childNodes`。其中保存着类数组对象 NodeList，能够动态反映 DOM 的结构变化。

访问保存在 NodeList 中的节点的两种方法：

<pre>
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
</pre>

**父节点：** `parentNodes`。

**兄弟节点：** `nextSibling`、`previousSibling`。

检测某节点是否有子节点，可以用`hasChildNodes()`方法。

###操作节点

`appendChild(要插入的节点)`：在 childNodes 列表末尾添加一个节点。若节点已在文档中，则将它移动到新位置。该方法返回新增节点。

`insertBefore(要插入的节点,参照节点)`：在 childNodes 列表特定位置插入节点。新插入节点将变为参照节点的前一个兄弟节点（即 previousSibling）。该方法返回新增节点。

`replace(要插入的节点,要移除的节点)`： 替换节点。该方法返回要移除的（被替换的）节点。

`removeChild(要移除的节点)`： 该方法返回被移除的节点。

`cloneNode(布尔值)`：创建调用该方法的节点的副本。参数为 true，深复制，即复制节点及整个子节点树；参数为 false，浅复制，即只复制节点本身。节点副本归属于文档，没有指定的父节点。

`normalize()`： 处理文档树中的文本节点。

##节点类型

###Document 类型

document 对象是 window 对象的一个属性，表示整个 HTML 页面。


####Document 类型的特征：

**nodeType:**	9

**nodeName:**	"#document"

**nodeValue:**	null

**parentNode:**	null

**ownerDocument:**	null

####关于文档的子节点：

<pre>
document.documentElement	//取得对&lt;html>的引用
document.body	//取得对&lt;body>的引用
document.doctype	//取得对&lt;!DOCTYPE>的引用
</pre>

其中，浏览器对`document.doctype`的支持有很大差异，因而该属性用处有限。

####关于文档信息：

<pre>
document.title	//取得（设置）文档标题
document.URL	//取得完整URL
document.domain	//取得（设置）域名
document.referrer	//取得来源页面的URL
</pre>

####关于查找方法：

<pre>
1. document.getElementById()	//返回严格匹配的相应元素ID||null
</pre>

在命名元素 ID 时，尽量不与表单的 name 属性重复。否则某些情况将获取错误的元素。

<pre>
2. document.getElementsByTagName()	//返回动态集合，HTMLCollection对象
</pre>

通过 name 特性，可以取得集合中的项：

假设有`<img src="she.gif" name="sheImg">`

<pre>
var imgs = document.getElementByTagName("img");
//字符串索引访问
var sheImg = imgs.namedItem("sheImg");
//方括号语法访问
var sheImg = imgs["sheImg"];
</pre>

<pre>
3. document.getElementByName()	//HTMLDocument独有方法，返回给定name所有元素
</pre>

####关于文档写入：

4个方法，皆接收一个字符串参数：

1. 	`write()`	原样写入
2. `writeln()`字符串末尾加换行符（\n）
3. `open()`	打开网页输出流
4. `close()`	关闭网页输出流

###Element 类型

####Element 类型的特征

**nodeType:**	1

**nodeName:**	元素的标签名（与 tagName 返回相同值）

**nodeValue:**	null

**parentNode:**	Document || Element

####HTML元素属性

每个 HTML 元素都有以下特性：

`id`

`title`，有关元素的附加说明信息，一般通过工具条可提示（鼠标悬浮可见）。

`lang`，元素内容的语言代码，很少使用。

`dir`，语言的方向。"ltr"，"rtl"。

`className`

####操作特性

操作特性的 DOM 方法主要有 3 个，可以针对包括以 HTMLElement 类型属性定义的任何特性使用。

有`<div id="aDiv"></div>`，取得该节点`var div = document.getElementById("aDiv");`。

1. `getAttribute(实际特性名)`
	
	有两类特殊特性：style，onclick。style 通过`getAttribute()`访问，返回 CSS 文本；通过属性访问则返回一个对象。onclick 通过`getAttribute()`访问，返回相应代码字符串；通过属性访问则返回 JavaScript 函数。开发时通常用属性操作 DOM。
	
	<pre>
	alert(div.getAttribute("id"));	//"aDiv"
	</pre>
	
2. `setAttribute(要设置的特性名,要设置的特性值)`

3. `removeAttribute(实际特性名)`

	删除特性和特性的值。IE6 以前不支持此方法。

P.S. 特性名称不区分大小写。

####创建元素

用`document.createElement(要创建元素的标签名)`方法可以创建新元素。下面是它的两种示例用法：

<pre>
var div = document.createElement("div");
</pre>

<pre>
var div = document.createElement("&lt;div id=\"aDiv\" class=\"divs\"></div>");
</pre>

###Text 类型

文本节点，包含纯文本，可以包含转义后的 HTML 字符，不能包含 HTML 代码。

####Text 类型的特征

**nodeType:**	3

**nodeName:**	"#text"

**nodeValue:**	节点所包含的文本（与 data 属性访问结果相同）

**parentNode:**	Element

####操作文本节点

`document.createTextNode()`	创建新文本节点。

`包含多个文本节点的父元素.normalize()`	合并相邻文本节点。

##测试

本文代码的演示效果，请移步[这里](http://blog.ilanyy.com/example/nodeType/)。
