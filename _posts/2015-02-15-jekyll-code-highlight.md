---
layout: post
title: "Jekyll中显示高亮代码"
keywords: ["Jekyll ","代码高亮"]
description: "Jekyll中显示高亮代码,pjax,多说无法加载"
category: "css"
tags: ["Jekyll"]
---
{% include JB/setup %}

最近将博客的主题更换为`3-Jekyll`，但发现这个主题不支持tab格式的代码，有些违背MarkDown的标准。于是选择了Google的Prettify，使用起来还是比较简单的，这里我做个笔记。也希望对想在jekyll里使用Prettify的coder一些帮助，

### 使用方法

由于[google-code-prettify](https://code.google.com/p/google-code-prettify/)国内访问不了，所以你可以直接下载我准备好的，[点我下载](http://cdn.saymagic.cn/o_19e6ofgp3s931dh1pqd1qcu1u5s9.bz2)

解压之后如下：

![google-code-prettify](http://cdn.saymagic.cn/o_19e6oj65s1v32157eltj1if21qd9e.png)

这里面我们需要的是`prettify.css`,`prettify.js`两个文件，在我们的head标签里可以添加如下代码：

	<link rel="stylesheet" href="/prettify/prettify.css">
	<script src="/prettify/prettify.js"></script>
	<script type="text/javascript">
 	 $(function(){
   	 $("pre").addClass("prettyprint linenums");
    	prettyPrint();
  	});
	</script>
	
linenums表示是添加行号，如果你不想需要这个属性可以去掉。默认的代码效果个人感觉不是特别的美观，代码要花花绿绿的才好看，我们可以自己修改`prettify.css`文件来修改代码的颜色和风格。也可以参考我这个博客代码风格的css文件：[https://gist.github.com/saymagic/5f4be898a63f584488ae](https://gist.github.com/saymagic/5f4be898a63f584488ae)


### 需要注意问题

如果你在jekyll里使用了pjax的话，这个高亮效果可能显示不出来，因为它没有被加载，解决的办法是将这段代码：

	 $(function(){
   	 $("pre").addClass("prettyprint linenums");
    	prettyPrint();
  	});

放在pjax结束的回调函数里。

同样的现象也会出现在多说评论框无法加载，只有刷新界面后才会显示等问题。解决方法也和上面是一样。Google it and fix it。





