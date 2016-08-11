---
layout: post
title: "DOM中的节点类型"
keywords: ["JavaScript","DOM","js","节点类型","node"]
description: "详细介绍DOM中的节点类型"
category: "HTML"
tags: ["HTML","DOM","XML"]
---
{% include JB/setup %}

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

###节点关系

