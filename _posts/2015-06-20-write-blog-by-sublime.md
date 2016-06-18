---
layout: post
title: "Sublime Text 进阶使用"
keywords: ["Sublime Text ","前端工具"]
description: "使用Sublime来写MarkDown博客"
category: "Tool"
tags: ["Sublime Text"]
---
{% include JB/setup %}

>相信很多人和我一样喜欢书写MarkDown格式的文本。以前我一直在使用MacDown来编写，但当发现了Sublime后,我找到了更有意思的写作方式，我将结合多个Sublime插件，让我们写的文章既可以存储在Git上，又可以保存在印象笔记供自己随时查看。此文以[Sublime简明教程](http://blog.saymagic.cn/2015/01/01/sublime_text_concise_course.html)一文为基础，如果对Sublime不是很了解可以先去查看此文。

## 插件安装

在本文中，我们需要安装如下一些插件，插件的安装方法可以参考上一篇文章的最后部分:[Sublime简明教程](http://blog.saymagic.cn/2015/01/01/sublime_text_concise_course.html)。所需插件如下:

* Git:方便我们在Sublime中提交博客。
* MarkDownEditing:在Sublime中编辑MarkDown文件。
* MarkDownPreview:在浏览器中预览MarkDown文件效果。
* SublimeTmpl:可以生成MarkDown文件模板。
* Evernote:保存文件到印象笔记.

## 开始前的准备

在安装好上述插件后，我们可以进行下一步。但在使用Evernote插件之前，需要将Sublime与我们的印象笔记关联，步骤如下：

* 打开 https://app.yinxiang.com/api/DeveloperToken.action ,然后登陆，进入如下界面:

![](http://cdn.saymagic.cn/o_19o8gssn51gtufvp1i6orj8niv9.png)

* 点击`Revoke your developer token`重新授权应用（若之前没有授权此步可能没有）:

![](http://cdn.saymagic.cn/o_19o8h0mv4el51c3iksbnlj1ef6e.png)

* 点击`Create a developer token`,然后会获得 Developer Token 和 NoteStore URL：

![](http://cdn.saymagic.cn/o_19o8h50f4dvv1r39187i1mrkc0vj.png)

* 记住上面的两个值。

* 打开 Sublime Evernote 插件的设置文件 Preferences > Package Settings > Evernote >Settings - User

* 将上面获取到的信息复制到相应的位置, 格式是：
    {
        "noteStoreUrl": " NoteStore URL",
        "token": " Developer Token"
    }
* token 是以S=开头的一串字符串，noteStoreUrl 是一段 http地址，你需要手动将https替换成http

* 保存设置文件。

### 测试是否授权成功

接下来我们可以尝试是否授权成功，通过`shift+command+p`打开命令窗口,输入Evernote，就会看见Evernote的许多命令:

![](http://cdn.saymagic.cn/o_19o8hehne194a1iu4krs1vqm162o9.png)

可以点击`Evernote:list recent notes`,如果看到罗列出最新的笔记，则说明授权成功，我们就可以开始后面的工作

![](http://cdn.saymagic.cn/o_19o8hfgcb1siubvh1ghr1qnn1uvue.png)

## 引入模板

`SublimeTmpl`插件为多种文件格式提供了模板，它的地址是[https://github.com/kairyou/SublimeTmpl](https://github.com/kairyou/SublimeTmpl),但很可惜它没有提供md格式的模板，这就需要我们来自定义模板，首先在`Users/用户名/Library/Application\ Support/Sublime\ Text\ 3/Packages/SublimeTmpl\templates`目录下新建`md.tmpl`文件，里面填写自己的模板，例如我的模板如下：

    ---
    layout: post
    keywords: k
    description: d
    title: t
    categories: [笔记]
    tags: [笔记]
    group: archive
    icon: globe
    ---

接下来将`Default.sublime-commands`、`Default.sublime-keymap`、`Main.sublime-menu`、`sublime-tmpl.py`几个文件按照其本来的格式进行修改，例如，`Default.sublime-keymap`文件代表快捷键，我将此文件修改如下：

![](http://cdn.saymagic.cn/o_19oa03aec1l0bsqa12uij12119.png)

此时，我只需按下`ctrl+alt+m`,即可按照模板新建一个md文件:

![](http://cdn.saymagic.cn/o_19oa06snp1l4vm1s13261i67747e.png)

现在，我们可以`ctrl+s`进行保存为md文件，`MarkDownEditing`插件就会将此md文件进行高亮显示。

![](http://cdn.saymagic.cn/o_19oa0e8ol3mji5h19h11gkrbiej.png)

## 效果预览

现在我们可以在Sublime中高亮我们的md文档了，但是我们怎么预览呢？此时就需要我们的`MarkDownPreview`插件登场了，该插件没有提供默认的快捷键，但提供了命令模式，打开命令模式,搜索`markdownpreview`，然后选择`MarkDownPreview preview in browser`：

![](http://cdn.saymagic.cn/o_19oa0tf0o18tju571bma1fjah0ge.png)

接着，选择你希望显示的模式，比如github,然后浏览器就会打开然后显示此md文件。

![](http://cdn.saymagic.cn/o_19oa6dob2mpeg8v13j81ckpap9.png)

需要提的一点是，当我们再次修改md文件，我们就无需通过上述命令来查看效果，直接刷新刚刚的页面即可。

## 存储到印象笔记

当我们写完博客后，我们可以通过如下步骤将此文件存储到印象笔记:

* 打开命令模式，搜索`ese`,就会找到`Evernote send to Evernote as new note`命令:

![](http://cdn.saymagic.cn/o_19oa1h7j41a5t11t0usqnbsda9e.png)
 
点击后会显示让我们选择存放在哪个笔记本，我们选择后就会自动存放到我们的印象笔记中，但此时你会发现一个问题，文件不高亮显示了，别急，此时文件按照`Evernote`格式显示，我们改回`MarkDown`文件即可,更改的地方在编辑器右下角:

![](http://cdn.saymagic.cn/o_19oa1s2bi1dsh1qur1co7nijqnbj.png)

我们再去我们的印象笔记看一下，你会发现存储在印象笔记中的格式已经是排版过的效果，很赞！

![](http://cdn.saymagic.cn/o_19oa1ublqn5dfeo1f2f157lcok9.png)

## 保存到Git仓库

现在，我们其实已经解决一个印象笔记不能使用`MarkDown`格式书写的问题，甚至有了这个插件后在我们对印象笔记要求不高的情况下完全可以卸载客户端了。

但可能大家写完文章后保存到Git上，`Sublime`的`Git`插件就可以解决这个问题，我们写完文章后，选择菜单栏上的Tool项，里面会有`Git`的相关按钮，我们可以先`add`,然后`commit`，这样，此文就保存到了我们的本地git仓库里。

## 总结

至此，本文开始所说的几个功能都已完成，总的下来，我个人的感觉就是两个字：折腾。因为Sublime相比专业的MarkDown编辑器还有很多差距，但这又怎样，折腾本来就是程序员应有的特质，在下一篇Sublime文章中，我将带领大家继续折腾，一起编写一个有意思的Sublime插件。敬请期待。

