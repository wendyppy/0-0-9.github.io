---
layout: post
title: "Sublime Text 简明教程"
keywords: ["Sublime Text ","前端工具"]
description: "Sublime Text的简明教程，分为安装、设置、快捷键、终端使用、命令模式、Goto Anything、插件安装几个部分"
category: "Tool"
tags: ["Sublime Text"]
---
{% include JB/setup %}

##  安装Sublime Text

Sublime 的安装比较简单，我们可以直接去官网[http://www.sublimetext.com/](http://www.sublimetext.com/)，点击Download菜单，进入之后选择自己操作系统的进行下载安装即可。安装完成后我们可以打开，测试效果如下：

![http://cdn.saymagic.cn/150101214652.41.59.png](http://cdn.saymagic.cn/150101214652.41.59.png)

可以发现，Sublime Text 拥有及其简单无公害的界面.

##  对Sublime Text进行一些设置

Sublime编辑器的可拓展性非常强，它通过配置文件的形式来对整个编辑器进行设置，因此，我们只需修改相应的配置文件即可修改Sublime 的许多特性，比如快捷键等等。这里我简单介绍怎样进行设置。

Sublime默认有个特别蛋疼的是每次打开一个文件都会新建一个窗口，特别让人抓狂，我们接下来就通过配置文件将其修改掉。首先，选中左上角的Sublime Text -> Preference  -> “Preferences.sublime-settings”，即可打开配置文件。按command+F 搜索 “open\_files\_in\_new\_window”，然后把true修改为false即可。

如果你上面的操作没有任何问题，那么说明你当前的Sublime版本号是2（或者是3修改这个bug了）。因为在Sublime 3里面有个小bug，就是你发现这个文件保存不了，不会生效。原因是这个文件的存放路径不存在。解决的方案就是我们自己来创建。

Preferences.sublime-settings文件的路径应为`/Users/用户名/Library/Application\ Support/Sublime\ Text\ 3/Packages/Default`,但是Sublime text 3的`/Users/用户名/Library/Application\ Support/Sublime\ Text\ 3/Packages/`目录下没有`Default`文件夹，我们只需要进入Package目录下，自己创建一个Default文件夹即可。接着重复我们刚才的操作就不会出现问题了。


##  Sublime Text 的快捷键

Sublime Text的快捷键还是比较符合正常人思维的，基本和eclipse、notepad＋＋、之类的保持相同。这里不做过多的解释。想说的是我们可以通过配置文件来修改我们需要的快捷键。选中左上角的Sublime Text -> Preference  ->  Key Binding Default,即可打开Sublime Text的快捷键配置文件，在这里我们我们就可以修改我们符合自己习惯的快捷键。

快捷键的文件名为`Default (OSX).sublime-keymap`,与上面提到的`Preferences.sublime-settings`同在Default目录下。因此如果你没有设置成功，请参考上一步。


##  配合终端的使用


当我们想要用Sublime Text 打开一个文件的时候，我们首先需要找到文件，然后右键选择用Sublime Text打开，但对于习惯终端操作的人来说不是很方便，没关系，Sublime Text提供了终端打开文件的功能。Sublime Text的终端命令为`subl`,但需要注意的是`subl`命令默认不在环境变量里，所以我们需要将其添加到环境变量，`subl`的位置为`/Applications/Sublime Text.app/Contents/SharedSupport/bin
`,我们需要讲这个路径添加到.bash_profile文件里。添加的方法我以前的文章也提过，这里不再重复，不会的话可以google。

完成上述操作后，我们就可以在终端使用Sublime Text 打开文件了：

	subl fileName        //打开文件
	subl folderName		 //打开文件夹
	subl .				 //打开当前目录
	


##  命令模式

习惯了Unix系列操作系统的人往往会觉得过于可视化操作会显得很low。所以让很多人喜欢Sublime Text的另一个原因是Sublime Text 提供了命令模式操作，提到命令模式我们往往会想到VIM,但Sublime Text 的命令模式要比VIM 的好用的多。

我们可以通过快捷键`command+shift+p`来打开命令模式：

![http://cdn.saymagic.cn/150101222824.28.03.png](http://cdn.saymagic.cn/150101222824.28.03.png)

我们可以在上图中的输入框里输入我们需要的命令，比如，我想拷贝当前文件的路径，输入`copy`之后，选择`File:copy path`选项之后，当前文件的路径就已经复制到了系统的剪切板上。

Sublime Text的命令模式支持模糊匹配，比如我们输入`cp`回车后可以直接实现上面的拷贝当前文件的路径功能（因为cp模糊匹配了File:`c`opy `p`ath）。

Sublime Text的命令模式功能很强大，大家可以随机的输入两个字母来模糊匹配一些命令，这里就不在一一罗列。

##  Goto AnyThing

当我们在运作一个大型项目的时候，如果文件目录很多层，文件查找是一个很头疼的问题，不过还好，Sublime Text里有一个叫做`Goto AnyThing`的功能，我们通过快捷键`command+p`打开`Goto AnyThing`窗口(该窗口和命令模式的窗口很相似，不过不是同一个)，在输入框中输入我们想要打开的文件模糊名称即可，Sublime Text会为我们查找出符合的文件。进而方便我们快速打开文件.

比较有意思的是Goto AnyThing不仅可以用于快速打开文件，还可以快速查看文件内部结构，我们`command+p`打开Goto AnyThing窗口后，输入`@`字符，就会出现当前文件的结构，如js文件会列出所有方法，md文件会列出大纲。

![http://cdn.saymagic.cn/150101231942.19.27.png](http://cdn.saymagic.cn/150101231942.19.27.png)

##  PackageControl

Sublime的强大之处在于它拥有非常多的插件来供我们使用，但这么多的插件没有一个统一的安装入口势必会造成很大的麻烦，因此具有了PackageControl，通过PackageControl我们可以方便的安装和卸载插件。

### 安装PackageControl

PackageControl是通过sublime内置的一个console来安装。首先我们打开console(view->Show Console 或者快捷键 control+ `)。下图中最下面的输入框就是我们输入内容的地方：

![http://cdn.saymagic.cn/150101212632.25.26.png](http://cdn.saymagic.cn/150101212632.25.26.png)

接下来，如果你的sublime版本数是`2`，则输入
	
	import urllib2,os,hashlib; h = '2deb499853c4371624f5a07e27c334aa' + 'bf8c4e67d14fb0525ba4f89698a6d7e1'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
	
进行安装。如果你的版本数是`3`，则复制如下内容回车安装：

	import urllib.request,os,hashlib; h = '2deb499853c4371624f5a07e27c334aa' + 'bf8c4e67d14fb0525ba4f89698a6d7e1'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
	
注意的是，需要本地装有python环境.

## 关于PackageControl

PackageControl可以安装哪些插件呢？我们可以前往PackageControl的官网[https://packagecontrol.io/](https://packagecontrol.io/)进行查看，上面我们的两端安装代码也是来自这个网站，地址为：[https://packagecontrol.io/installation](https://packagecontrol.io/installation).（如果上面的代码安装失败，请以官网上的代码为准。）

##  通过PackageControl安装插件

当我们安装完成PackageControl之后，就可以来安装各种插件来提升我们Sublime Text 的功能了。这里我介绍两个插件，剩余的大家可以到官网各取所需。


###  创建文件－-advancedNewFile插件

当我们在 Sublime Text 编辑器里我们可以通过快捷键`command+n`来新建一个文件，然后`command+s`进行弹出保存框，填写文件名进行保存。还是老问题，麻烦！！我们接下来就通过安装`advancedNewFile`插件来提升我们在Sublime Text编辑器下的创建文件速度。

我们首先打开命令模式（command＋shift＋p），输入pci（Package Control:Install Package的简写，我们可以通过输入pci快速的打开Package Control的安装界面）后回车,我们在新的文本框里输入`advancedNewFile`后回车，稍等一会这个插件就会自动安装完成，Sublime Text 会打开一个新的窗口，告诉我们安装完成了，界面如下：

![http://cdn.saymagic.cn/150101224810.46.45.png](http://cdn.saymagic.cn/150101224810.46.45.png)

advancedNewFile是怎样提高新建文件速度呢？我们可以使用快捷键`command+alt+n`，Sublime Text底部会弹出输入框:

![http://cdn.saymagic.cn/150101225117.50.59.png](http://cdn.saymagic.cn/150101225117.50.59.png)

我们只需在这个输入框里输入我们需要新建的文件名回车即可（我们甚至可以带路径）。默认情况下文件会存储在当前目录，如果当前没有目录，会存储在用户的家目录。


###  增强的sidebar--SideBarEnhancements
	
当我们用sublime打开一个文件夹时，会在sublime试图框的左侧出现一个sidebar，以此方便我们可以通过点击的方式快速打开文件。但这个sidebar功能很少，不能满足日常需求。只有如下三个选项：

![http://cdn.saymagic.cn/150101213519.05.52.png](http://cdn.saymagic.cn/150101213519.05.52.png)

PackageControl中有一款名字叫做SideBarEnhancements的插件可以增强sidebar的功能。打开命令模式－>进入pci界面->输入SideBarEnhancements回车安装：

![http://cdn.saymagic.cn/150101213847.gif](http://cdn.saymagic.cn/150101213847.gif)

安装完成之后，效果如下：

![http://cdn.saymagic.cn/150101214015.gif](http://cdn.saymagic.cn/150101214015.gif)


