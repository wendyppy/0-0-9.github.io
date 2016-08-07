---
layout: post
title: "BOM中的location对象"
keywords: ["JavaScript","BOM","js","setTimeInterval","location"]
description: "详细解读JavaScript BOM中的location对象"
category: "JavaScript"
tags: ["JavaScript","BOM"]
---
{% include JB/setup %}

location 是最有用的 BOM 对象之一，它既是 window 对象的属性，也是 document 对象的属性。也就是说，`window.location`和`document.location`引用的是同一个对象。

##location对象的属性

<table>
  <tr>
    <th style="width:30%">属性</th>
    <th>描述</th>
  </tr>
  <tr>
    <td>hash</td>
    <td>设置或返回从井号 (#) 开始的 URL（锚）。</td>
  </tr>
  <tr>
    <td>host</td>
    <td>设置或返回主机名和当前 URL 的端口号。</td>
  </tr>
  <tr>
    <td>hostname</td>
    <td>设置或返回当前 URL 的主机名。</td>
  </tr>
  <tr>
    <td>href</td>
    <td>设置或返回完整的 URL。</td>
  </tr>
  <tr>
    <td>pathname</td>
    <td>设置或返回当前 URL 的路径部分。</td>
  </tr>
  <tr>
    <td>port</td>
    <td>设置或返回当前 URL 的端口号。</td>
  </tr>
  <tr>
    <td>protocol</td>
    <td>设置或返回当前 URL 的协议。</td>
  </tr>
  <tr>
    <td>search</td>
    <td>设置或返回从问号 (?) 开始的 URL（查询部分）。</td>
  </tr>
  </table>
  
  举个 🌰 ：
  
  有个这样的网址1☞<code class="txt">https://github.com/jnotnull/JavaScript-Sturcture/wiki/Angular-2.0-%E5%92%8C-1.x%E6%AF%94%E8%BE%83#dom</code>，它的各 location 属性见下图：
  
  ![](http://cdn.saymagic.cn/o_1apii51m71omu10841rp21paijl39.png)
  
网址2☞<code class="txt">https://github.com/jnotnull/JavaScript-Sturcture/search?utf8=%E2%9C%93&q=angular</code>

![](http://cdn.saymagic.cn/o_1apiid1i814mr447at8an4v1oe.png)
  
  详细表格见[此处](http://blog.ilanyy.com/example/location/)。##location对象的方法

###assign()

加载新文档。

立即打开新URL并在浏览器历史记录生成一条记录：

<pre>
location.assign(URL);
</pre>

等同于：

<pre>
window.location  = URL;
</pre>

或者：

<pre>
location.href = URL;
</pre>

另外，修改 location 对象的其他属性也可以改变当前加载的页面。每次修改 location 的属性（hash 除外），页面都会以新 URL 重新加载。