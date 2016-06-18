---
layout: post
title: "客户端 less 安装"
keywords: ["less ","css"]
description: "客户端 less 安装"
category: "Frame"
tags: ["less"]
---
{% include JB/setup %}

less是一种css预处理语言，为 css 添加了一些编程特性；如允许变量（variables）、混合（mixins）、函数（functions）等，从而使 css 更简洁、适应性和可维护性更强。
##安装
> 在浏览器中使用 less.js 开发是很好的，但不推荐用于生产环境中。

浏览器端（客户端）使用是在使用less开发时最直观的一种方式。如果是在生产环境中，尤其是对性能要求比较高的场合， 建议使用 node 或者其它第三方工具先编译成 css 再上线使用 。
在[官网](http://lesscss.org/)下载一个 javascript 脚本文件“`less.js`”，然后在我们需要引入 less 源文件的 html 的 `<head>` 中加入如下代码：

    <link rel="stylesheet/less" type="text/css" href="文件路径/styles.less">
    <script src="文件路径/less.js" type="text/javascript"></script>

注意：<br />
1. 在引入的“`.less`”文件中，“link”的“rel”属性要设置为“`stylesheet/less`”。<br>
2. 确保包涵 `.less` 的样式表在` less.js` 脚本之前。<br>
3. 当引入多个 `.less `样式表时，它们都是独立编译的。所以，在每个文件中定义的变量、混合、命名空间都不会被其它的文件共享。
##Browser Options （浏览器端设置参数）
可以在引入 `<script src="less.js"></script> ` 之前通过创建一个全局 less 对象的方式来指定参数，例如：

    <!-- set options before less.js script -->
    <script>
      less = {
    env: "development",
    async: false,
    fileAsync: false,
    poll: 1000,
    functions: {},
    dumpLineNumbers: "comments",
    relativeUrls: false,
    rootpath: ":/a.com/"
      };
    </script>
    <script src="less.js"></script>

或为了简化它们可以在 script 和 link 标记上设置属性。

    <script src="less.js" data-poll="1000" data-relative-urls="false"></script>

    <link data-dump-line-numbers="all" data-global-vars='{ myvar: "#ddffee", mystr: "\"quoted\"" }' rel="stylesheet/less" type="text/css" href="less/styles.less">

注意：以上三种配置参数的优先级为：link标签 > script标签 > 全局对象。

<br>
**async** (类型：`Boolean`  默认：`false`)<br>
async 参数是用来判断是否异步导入文件。<br>

**dumpLineNumbers** (类型：`String` 参数：`''| 'comments'|'mediaquery'|'all' `默认：`''`)<br>
设置 dumpLineNumbers 直接输出源行信息到编译好的 css 文件，有利于调试指定行。<br>
`comments` 参数用于输出用户可以理解的内容，而 `mediaquery` 使用 Firefox 一个扩展来解析 css 和抽取信息。

**env** (类型：`String`   默认：取决于页面的URL)<br>
可以在`development`或是`production`环境下运行。<br>
在`production`环境下，css被缓存在本地，消息和信息不能输出到 console。<br>
文档的URL开头是`file:// `，或是在本地机器中，或是有不标准端口，env 的参数将自动设置为`development`。<br>
例如：<br>
    less = { env: 'production' };

**errorReporting** (类型:：`String` 参数：`html|console|function`  默认：`html`)<br>
在 less 编译失败时候，errorReporting 会设置错误报告的方法。

**fileAsync** (类型：`Boolean`	默认：`false`)<br>
使用文件协议访问页面时异步加载导入的文件。

**functions** (类型： `object`)<br>
在 functions 这个对象中，key 作为函数的名字。例如：<br>

    less = {
    	functions: {
    		myfunc: function() {
  			  return new(less.tree.Dimension)(1);
  		  }
    	}
    };

functions 可以像内置的 less 函数一样使用。<br>

    .my-class {
      border-width: unit(myfunc(), px);
    }

**poll** (类型：`Integer`	默认：`1000`)<br>
在监视模式下，每两次请求之间的时间间隔（ms）。

**relativeUrls** (类型：`Boolean` 默认：`false`)<br>
是否调整相对路径。如果为 false，则 url 已经是相对于根目录的 less 文件。

**globalVars** (类型：`object`)<br>
全局变量列表注入代码。”字符串“类型的变量必须显式地包含引号。<br>

    less.globalVars = { myvar: "#ddffee", mystr: "\"quoted\"" };
这个选项定义了一个可以被文件引用的变量。这个变量也可以在文件中重新定义。

**modifyVars** (类型：`object`)<br>
与 globalVars 参数含义相反。它将会在你文件最后定义，这意味着它将重写在文件中的定义。

**rootpath**  (类型:：`String` 默认：`false`)<br>
设置根目录，所有的 less 文件都会以这个目录开始。

##监视模式
如果使用监视模式，则配置参数的 `env` 为 `development` 。然后在`less.js`文件加载之后调用`less.watch()`，如下：

    <script>less = { env: 'development'};</script>
    <script src="less.js"></script>
    <script>less.watch();</script>

 注意：
如果启动了监视模式，则浏览器会不断请求 less 文件，根据` Last-Modified `参数判断是否重新渲染页面，这会造成很大的性能消耗，所以在线上不要开启监视模式。如果是开发环境，这方便了我们观察效果。你也可以在 href 后面加上`'#!watch'`来触发监视模式。

----------

 完整demo：<br>

`reset.less`是重置浏览器默认样式，config.js是浏览器选项的配置参数，如下：

config.js

>     less = {
>     env: "development", // or "production"
>     async: false, // load imports async
>     fileAsync: false, // load imports async when in a page under a file protocol
>     poll: 1000, // when in watch mode, time in ms between polls
>     functions: {}, // user functions, keyed by name
>     dumpLineNumbers: "all", // "comment" or "mediaQuery" or "all"
>     relativeUrls: false,// whether to adjust url's to be relative
>     // if false, url's are already relative to the entry less file
>     rootpath: ":/"// a path to add on to the start of every url resource
>     };

index.html

>     <!DOCTYPE html>
>     <html>
>     <head>
>     <meta http-equiv="Content-Type" content="text/html;charset=utf-8">
>     <link rel="stylesheet/less" type="text/css" href="./less/reset.less" />
>     <link rel="stylesheet/less" type="text/css" href="./less/styles.less" />
>     <script src="./js/config.js"></script>
>     <script src="./js/less-1.3.3.min.js"></script>
>     <script>less.watch();</script>
>     </head>
>     <body>
>     </body>
>     </html>