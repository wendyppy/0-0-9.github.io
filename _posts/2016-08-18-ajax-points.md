---
layout: post
title: "Ajax知识点"
keywords: ["Ajax","JavaScript","异步"]
description: "Ajax知识点一览"
category: "JavaScript"
tags: ["Ajax","JavaScript"]
---
{% include JB/setup %}

##Ajax概念

###同步

![](http://cdn.saymagic.cn/o_1ar5cmgh31ofl58ep9nnqvnf39.jpg)

###异步

![](http://cdn.saymagic.cn/o_1ar5cnifc1icuhu81ulqs5l19goj.jpg)

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

HTTP 请求的组成：

1. 请求的方法或动作，GET（一般用于信息的获取，查询） | POST（信息的修改、新建）
2. 正在请求的 URL，即请求的地址
3. 请求头，包含客户环境信息、身份验证信息等
4. 请求体，即请求正文，可包含用户提交的查询字符串、表单信息等（请求头和请求体之间会有一个空行）

HTTP 响应的组成：

1. 一个数字和文字组成的状态码，用于显示请求成功还是失败
2. 响应头，包含服务器类型、日期时间、内容类型等服务器相关信息
3. 响应体，即响应正文

HTTP 状态码由三位数字构成，首位数字定义了状态码的类型：

`1XX`：信息类，表示收到 Web 浏览器的请求，正在进一步处理中

`2XX`：成功，表示用户请求被正确接收、理解、处理，如 200 OK

`3XX`：重定向，表示请求未成功，客户必须采取进一步动作

`4XX`：客户端错误，表示客户提交的请求有误，如 404 Not Found

`5XX`：服务器错误，表示服务器不能完成对请求的处理，如 500

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

###XMLHttpRequest发送请求

<pre>
open(method, url, async)
</pre>

**method：**发送请求方法。不区分大小写，一般用大写。

**url：**请求地址。可以使用相对文档的地址，也可用绝对地址。

**async：**请求同步（false）/异步（默认，true）

<pre>
//把请求发送到服务器
send(string)
</pre>

GET 请求，可不填写参数或参数填写为 null；POST 请求一定填写。

几个 🌰：

<pre>
request.open("GET", "get.php", true);
request.send();
</pre>
<pre>
request.open("POST", "post.php", true);
request.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); //位置不能变
request.send("name=Selina&age=18");
</pre>

###XMLHttpRequest获取服务器响应

####获取响应值

responseText：获得字符串形式的响应数据

responseXML：获得 XML 形式的响应数据

status 和 statusText：以数字和文本形式返回 HTTP 状态码

getAllResponseHeader()：获取服务器所有的响应报头

getResponseHeader()：查询响应中的某个字段值

####响应成功时获得通知

readyState 属性：

`0`：请求未初始化，open 还没有调用

`1`：服务器连接已经建立，open 已经调用

`2`：请求已经被接受，即服务器已收到头信息

`3`：请求正在处理当中，即服务器已接收到头和主体

`4`：请求已完成，且响应就绪，即响应已完成

`onreadystatechange`事件：监听 readyState 属性。

<pre>
var request = new XMLHttpRequest();
request.open("GET", "get.php", true);
request.send();
request.onreadystatechange = function() {
    if (request.readyState === 4 && request.status === 200) {
        //do something... request.responseText
    }
};
</pre>

##json

使用 Ajax 传递数据，基本都采用 json 格式。

json 介绍：[什么是 json](http://json.cn/json/wiki.html)

在线 json 校验工具：[jsonlint](http://jsonlint.com/)，[json.cn](http://json.cn/)

某项目 JavaScript 文件节选：

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
    }
};

var searchBtn = document.getElementById("searchBtn");
var searchFn = function() {
    var keywordVal = document.getElementById("keyword").value;
    var request = new XMLHttpRequest();
    request.open("GET", "server.php?number=" + keywordVal);
    request.send();
    request.onreadystatechange = function() {
        if (request.readyState === 4) {
            if (request.status === 200) {
                var response = JSON.parse(request.responseText);
                if (response.success) {
                    document.getElementById("searchResult").innerHTML = response.msg;
                } else {
                    document.getElementById("searchResult").innerHTML = "error: " + response.msg;
                }
            } else {
                alert("出错！错误代码" + request.status);
            }
        }
    };
};

var saveBtn = document.getElementById("saveBtn");
var saveFn = function() {
    var request = new XMLHttpRequest();
    request.open("POST", "server.php");
    var data = "name=" + document.getElementById("staffName").value +
        "&number=" + document.getElementById("staffNum").value +
        "&sex=" + document.getElementById("staffSex").value +
        "&job=" + document.getElementById("staffJob").value;
    request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
    request.send(data);
    request.onreadystatechange = function() {
        if (request.readyState === 4) {
            if (request.status === 200) {
                var response = JSON.parse(request.responseText);
                if (response.success) {
                    document.getElementById("createResult").innerHTML = response.msg;
                } else {
                    document.getElementById("createResult").innerHTML = "error: " + response.msg;
                }
            } else {
                alert("出错！错误代码" + request.status);
            }
        }
    };
}

window.onload = function() {
    EventUtil.addHandler(searchBtn, "click", searchFn);
    EventUtil.addHandler(saveBtn, "click", saveFn);
};
</pre>

[例子](http://blog.hardworking.top/example/json/)

##jquery中的Ajax

<pre>
$.ajax([settings])
</pre>

**type：**类型，GET | POST，默认 GET

**url：**发送请求的地址

**data：**是一个对象，连同请求一块发送到服务器中，主要是 POST 请求使用

**dataType：**预期服务器返回的数据类型。若不指定，jquery 将根据 HTTP 包 MIME 信息智能判断，可设置为`json`。

**success：**是一个方法，请求成功后的回调函数。参数传入返回后的数据。

**error：**是一个方法，请求失败后调用该函数。函数参数需传入 XMLHttpRequest 对象。

用 jquery 改写上面的 JavaScript 文件：

<pre>
$(document).ready(function() {
    $("#searchBtn").click(function() {
        var keywordVal = $("#keyword").val();
        $.ajax({
            url: "serverjson.php?number=" + keywordVal,
            type: "GET",
            dataType: "json",
            success: function(data) {
                if (data.success) {
                    $("#searchResult").html(data.msg);
                } else {
                    $("#searchResult").html("error: " + data.msg);
                }
            },
            error: function(xhr) {
                alert("出错！错误代码" + xhr.status);
            }
        });
    });
    $("#saveBtn").click(function() {
        $.ajax({
            url: "serverjson.php",
            type: "POST",
            data: {
                name: $('#staffName').val(),
                number: $('#staffNum').val(),
                sex: $('#staffSex').val(),
                job: $('#staffJob').val()
            },
            dataType: "json",
            success: function(data) {
                if (data.success) {
                    $("#createResult").html(data.msg);
                } else {
                    $("#createResult").html("error: " + data.msg);
                }
            },
            error: function(xhr) {
                alert("出错！错误代码" + xhr.status);
            }
        });
    });
});
</pre>

##跨域

先看看一个域名地址的组成：

![](http://cdn.saymagic.cn/o_1ar855up61k963cgpvv147017goe.jpg)

当（图中标红部分）协议、子域名、主域名、端口号任意一个不同时，都算作不同域。不同域之间相互请求资源，叫“跨域”。

JavaScript “同源策略”，出于安全考虑，不允许跨域调用其他页面的对象。

###服务端代理

同源服务器请求跨域页面，属于后端技术。不做讨论。

###JSONP

只对 GET 请求起效果。（不支持 POST 请求）

假设上面 jQuery + Ajax 的示例代码中的 GET 请求想访问 跨域的资源 serverjson.php，为使代码支持跨域，可以这样改动：

1. 对 JavaScript 文件：将`dataType: "json"`改为`dataType: "jsonp"`，增加`jsonp: "cb123"`。其中，cb123 是自定义的名称。
2. 对 serverjson.php 文件：在相应的事件处理方法中，增加`$jsonp = $_GET["cb123"];`，改写返回值`$result = '{"success":false,"msg":"没有找到员工。"}';`为`$result = jsonp.'({"success":false,"msg":"没有找到员工。"})';`；继续改写返回值`$result = '{"success":true,"msg":"找到员工：员工编号：' . $value["number"] .'，员工姓名：' . $value["name"] .'，员工性别：' . $value["sex"] .'，员工职位：' . $value["job"] . '"}';`为`$result = jsonp.'({"success":true,"msg":"找到员工：员工编号：' . $value["number"] .'，员工姓名：' . $value["name"] .'，员工性别：' . $value["sex"] .'，员工职位：' . $value["job"] . '"})';`。

###XHR2

HTML5 提供的 XMLHttpRequest Level2 已经实现了跨域访问和其它一些功能。可惜 IE10 以下版本并不支持。

使用时只需微改服务器端（如果你的服务端语言是 php 或者 java）：

<pre>
//*表示允许全部域名跨域请求
header('Access-Control-Allow-Origin:*');
//*允许a，b网站跨域请求
header('Access-Control-Allow-Origin:www.a.com,www.b.com');

header('Access-Control-Allow-Methods:POST,GET');
</pre>

查看某个网址的资源是否允许跨域访问，可以通过开发者工具打开`Network`标签，刷新页面，查看指定资源的请求头中的`Access-Control-Allow-Origin`属性。像这样 ↓↓：

![](http://cdn.saymagic.cn/o_1arb6rc2f1bengtm1v8v1s111ehp9.png)
