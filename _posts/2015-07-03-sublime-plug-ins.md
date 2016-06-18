---
layout: post
title: "Sublime Text3 插件备份"
keywords: ["Sublime Text3 ","前端工具"]
description: "Sublime Text3 插件备份"
category: "Tool"
tags: ["Sublime Text3"]
---
{% include JB/setup %}

Sublime Text3 是一款非常强大的前端工具。在此写上目前安装的各种插件，以作备份。(快捷键均为Windows环境下操作)
##插件
###1. Package Control
**安装**<br>
从菜单 `View` >`Show Console` 或者 ``Ctrl + ` ``快捷键，调出 console。按照 Sublime 版本，粘贴如下代码：

**Sublime Text 3**

    import urllib.request,os; pf = 'PackageControl.sublime-package'; ipp = sublime.installed_packages_path();urllib.request.install_opener( urllib.request.build_opener(urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf),'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace('','%20')).read())

**Sublime Text 2**

    import urllib2,os; pf='PackageControl.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs(ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf),'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( '','%20' )).read()); print( 'Please restart Sublime Text to finishinstallation')

可能由于各种原因，无法使用代码安装，那可以通过以下步骤手动安装 `Package Control`：

1. 点击`Preferences` > `BrowsePackages`菜单；

2. 进入打开的目录的上层目录，然后再进入`Installed Packages/`目录；

3. 下载 [Package Control.sublime-package](https://sublime.wbond.net/Package%20Control.sublime-package)并复制到`Installed Packages/`目录；

4. 重启SublimeText。

**使用方法**<br>
快捷键 ` Ctrl+Shift+P`（菜单 > `Tools`  > `Command Paletter`），输入` install` 选中`Install Package`并回车，输入或选择你需要的插件回车就安装了（注意左下角的小文字变化，会提示安装成功）。
###2. AdvacedNewFile
快速创建新文件。

**快捷键**：`Ctrl+Alt+N`

在多文件夹的项目开发中很便利，如输入`script/utils/API.js`回车就可以自动创建目录结构以及空文件。由于我们打开了`script/app.js`文件，可以直接输入`./utils/API.js`创建相对路径的文件结构。另外，对于已存在的目录可以使用Tab补全。创建出来的新文件会自动打开，并且会自动选择相应的语法，没有额外的工作。
###3. AutoFileName
css、js等文件，img、background的url等属性的文件路径自动补齐。
###4. ColorPicker

**快捷键**：`Ctrl+Shift+C`

convertToUTF8 和 ColorPicker 快捷键会产生冲突，convertoUTF8 的默认转换GBK的快捷键 和 ColorPicker 打开调色板的快捷键都是`ctrl+shift+c `。如果你两个插件都安装了的话，就需要进行自定义了，例如： 

解决方法：打开 Sublime Text  > `Preferences `> `Browse Packages`，找到ConvertToUTF8 文件夹并进入，找到对应操作系统的Default.sublime-keymap文件，直接修改成 
    [ 
    { "keys": ["ctrl+shift++alt+c"], "command": "convert_to_utf8", "args": {"encoding": "GBK", "stamp": "0" } } 
###5. CSS Format
**快捷键**

为了防止产生快捷键冲突，CSS Format 默认没有开启任何快捷键。

如果你想为特定的格式化命令定义快捷键，可参考 `Preferences` > `Package Settings` > `CSS Format ` >  `Key Bindings – Example`，将你需要的快捷键定义在` Preferences`  >  `Package Settings` > `CSS Format`  > `Key Bindings – User` 中。

注意：你需要始终编辑 `Key Bindings – User `文件，不要编辑 `Key Bindings – Example` 文件，否则下次插件更新会覆盖你的设置。

**保存时自动格式化**

打开 `Preferences` > ` Package Settings`  >  `CSS Format ` >  `Settings – Default`，将 `Settings – Default` 里的内容复制到` Preferences`  > ` Package Settings`  >  `CSS Format`  > `Settings – User` 中，然后将 format_on_save 的值改为 true。

注意：你需要始终编辑 `Settings – User `文件，不要编辑 `Settings – Default `文件，否则下次插件更新会覆盖你的设置。

配置项说明：

- `indentation`：缩进字符，默认为 "\t"，你可以设置为 "    "（4个空格）。
- `format_on_save`：保存时是否自动格式化，默认为 false，如需开启请改为 true
- `format_on_save_action`：自动格式化时执行的命令，默认设置为 "expand"，可选值有 "expand"、"expand-bs"、"compact"、"compact-ns"、"compact-bs"、"compact-bs-ns"、"compress"
- `format_on_save_filter`：文件筛选正则，默认为 "\\.(css|sass|scss|less)$"，只有文件名可以被此正则匹配的文件，才可以使用保存时自动格式化的功能。

###6. DocBlockr
代码注释自动生成。

**使用方法**

输入`/*`，`/**`然后回车或 Tab，还有很多用法，请参照 [https://sublime.wbond.net/packages/DocBlockr](https://sublime.wbond.net/packages/DocBlockr)

![](http://cdn.saymagic.cn/o_19pa6lqk11amk1a13hjm1vr3a2q9.gif)

![](http://cdn.saymagic.cn/o_19pa6n30v14n81pf91rnlc7kkme.gif)

###7. Emmet
编码快捷，前端必备

**使用方法**：
详情参考 [http://docs.emmet.io/cheat-sheet/](http://docs.emmet.io/cheat-sheet/)
###8. Gutter Color
1. 下载imagemagick [http://www.imagemagick.org/script/binary-releases.php#windows](http://www.imagemagick.org/script/binary-releases.php#windows)

	我的安装包→：
[http://vdisk.weibo.com/s/uJehmAky5Zh8w](http://vdisk.weibo.com/s/uJehmAky5Zh8w)

2.  打开 `Preferences` > `Package Settings` > `GutterColor` > `Settings – User`

	输入     {
    "convert_path" : "convert.exe"
    }

如果路径对了，即可看到介绍中的图片，每行前边有个代表本行中的颜色点。
![](http://cdn.saymagic.cn/o_19pbqgt0m190vbvl36rtacchh9.png)
配合 ColorPicker 插件，提高处理颜色效率。
###9. jQuery
jQuery 提示。
###10. JSFormat
 js 格式化。

**快捷键**: `Ctrl+Alt+F`
###11. SideBarEnhancements
侧边栏增强。可在 Sublime 编辑器中快捷打开在浏览器中显示效果。

配置：




1.`Preferences` > `Package Settings` > `Side Bar` > `Settings - User`

放置如下代码：

    {
    	"default_browser": "chrome", //one of this list: firefox, aurora, chrome, canary, chromium, opera, safari
    
    	"portable_browser": "C:\\Users\\009\\AppData\\Local\\Google\\Chrome\\Application\\chrome.exe"// for example:  C:/Program Files (x86)/Nightly/firefox.exe
    }

即可配置打开网页的默认浏览器。


2.在侧边栏中选择一个文件，右键 > `Open With` > `Edit Applications...`
在打开的 Side Bar.sublime-menu 文件中可配置其他 Open With 的应用类型。
###12. SublimeCodeIntel
这是一款代码提示插件，支持多种编程语言。安装时间较长，安装该插件后需要根据使用的编程语言进行配置。

本部分将以配置 JavaScript 语言的代码提示功能为例。

安装该插件后，点击 `Preferences` > `Browse Packages...`，在弹出的窗口中进入 `SublimeCodeIntel` > `.codeintel` > `config` ，拖入Sublime中打开。写入以下代码：

    {"JavaScript":{	"javascriptExtraPaths":[]}}

效果如图：![](http://cdn.saymagic.cn/o_19pcgihmj9pb1e1d1gka11fmjt79.png)
###13. Tag
HTML格式化。
###14. TrailingSpaces
高亮并可选去掉多余空格。

注：使用一段时间后，会发现该插件莫名不能用了。这时打开 `Preferences` > `Package Settings` >  `Trailing Spaces` > `Settings - User`，可看到里面有：

    {
    	"trailing_spaces_highlight_color": "",
    	"trailing_spaces_modified_lines_only": false
    }

把第一行改为：**` "trailing_spaces_highlight_color": "invalid" `** ，可解决。
###15. BufferScroll
Sublime 中，快捷键 `Ctrl+Shift+[` 可以折叠代码，`Ctrl+Shift+]` 释放。
前后对比如下图：

![](http://cdn.saymagic.cn/o_19qidredc179u164lua81tn9df99.png)

![](http://cdn.saymagic.cn/o_19qidv5iv123h17rnd5b13qo1v5qe.png)

但重启 Sublime 后，折叠效果并不能保存。这就需要 BufferScroll 来为我们保留代码的折叠状态。

###16. BracketHighlighter
在左侧高亮显示匹配的括号、引号和标签，能匹配`[] ,  () ,  {} ,  "" ,  '' , <tag></tag>`等甚至是自定义的标签，当看到密密麻麻的代码分不清标签之间包容嵌套的关系时，这款插件就能很好地帮你理清楚代码结构，快速定位括号，引号和标签内的范围。

###17. ConvertToUTF8
解决 Sublime 不支持 GBK 、GB2312 等编码的问题，支持 Sublime 打开 GB2312 编码的文件并提供其输入并编辑中文，在打开 GB2312 文件后会将其转换为 UTF-8 编码（这不会修改原始文件编码），对于输入和编辑的中文字符在使用 Sublime 保存后会将其转换为原始编码后再进行保存。

**注：**当文件不小心被保存为 UTF-8 ，而且变为乱码，恢复方法如下：
打开这个文件，并确认它的编码是 UTF-8 ，在菜单中选择 `File` > `Save with Encoding` > `Western(Windows 1252)` 。关闭后重新打开该文件即可恢复正常。

###18. IMESupport
解决 Sublime Text 中文输入法不能跟随光标的问题。目前只支持 Windows。

![](http://cdn.saymagic.cn/o_19r47jolk16av8nd1ug71a8b1qc3e.gif)



<br />
关于 Sublime Text3，这里有份中文文档 [http://feliving.github.io/Sublime-Text-3-Documentation/](http://feliving.github.io/Sublime-Text-3-Documentation/) ，可作更多了解。