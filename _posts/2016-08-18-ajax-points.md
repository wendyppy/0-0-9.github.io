---
layout: post
title: "Ajax知识点"
keywords: ["Ajax","JavaScript","异步"]
description: "Ajax知识点一览"
category: "JavaScript"
tags: ["Ajax","JavaScript"]
---
{% include JB/setup %}

##同步

![](http://cdn.saymagic.cn/o_1ar5cmgh31ofl58ep9nnqvnf39.jpg)

##异步

![](http://cdn.saymagic.cn/o_1ar5cnifc1icuhu81ulqs5l19goj.jpg)

###XMLHttpRequest对象

<pre>
var request;
if (window.XMLHttpRequest) {
    //IE7+,FF,Chrome,Opera,Safari
    request = new XMLHttpRequest();
} else {
    //IE5,IE6
    request = new ActiveXObject("Microsoft.XMLHTTP");
}
</pre>

###HTTP请求

HTTP 是无状态的协议（即服务端不保留连接的相关信息，无记忆）。

一个完整的 HTTP 请求包含以下步骤：

1. 建立 TCP 连接
2. Web 浏览器向 Web 服务器发送请求命令
3. Web 浏览器发送请求头信息
4. Web 服务器做出应答
5. Web 服务器发送应答头信息
6. Web 服务器向浏览器发送数据
7. Web 服务器关闭 TCP 连接