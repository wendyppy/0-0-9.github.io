---
layout: post
title: "Sublime Text3 个人设置"
keywords: ["Sublime Text3 ","前端工具"]
description: "Sublime Text3 个人设置"
category: "Tool"
tags: ["Sublime Text3"]
---
{% include JB/setup %}

上一次写了一些插件的备份，这次来记录下个人设置。

在菜单栏中选择 `Preferences` > `Settings - User` ，重写入以下内容：

    {
    	"caret_style": "blink",//光标效果
    	"font_size": 18,
    	"highlight_line": true,
    	"ignored_packages":
    	[
    		"Vintage"
    	],
    	"fold_buttons": true,
    	"fade_fold_buttons": false,// 不管鼠标在不在行号边栏，代码折叠按钮一直显示
    	// "save_on_focus_lost": true,//焦点丢失后自动保存
    	"show_encoding": true,//开启后，在编辑器的右下角会显示当前文件的编码
	"show_line_endings": true,
    	"trim_trailing_white_space_on_save": true,//保存的时候把无用的空格去掉,去掉的是每一行文本最后面的空格
    	"show_full_path": true,//显示全路径,默认
    	"bold_folder_labels": true,//这个设置会让文件夹的名称加粗一些，让你更好辨认
    	"draw_white_space": "all",//显示所有空格，"none"|"selection"|"all"
    	// "sublime_enhanced_keybindings": true, //转到上一次编辑的地方用
    	"highlight_modified_tabs": true,//标注出一个窗口里修改过的文件,给当前项目中没有存档的文件更多注意力
    	"word_wrap": true,//文字根据屏幕大小自动换行，防止水平滚动 true | false | "auto"  	
    	"line_padding_top": 2,
    	"line_padding_bottom": 2,
    
    	"theme": "Spacegray.sublime-theme",
      	"color_scheme": "Packages/Theme - Spacegray/base16-ocean.dark.tmTheme",
    	"spacegray_tabs_font_large": true,
    	"spacegray_tabs_large": true,
    	"spacegray_tabs_auto_width": true,
    	"spacegray_sidebar_font_xlarge": true,
    	"spacegray_sidebar_tree_large": true, 
    	"enable_tab_scrolling": false
    }


复制版（直接copy上面内容也可以的，因为 sublime 会在重启后自动去掉注释并把内容按首字母排序）：

    {
    	"bold_folder_labels": true,
    	"caret_style": "blink",
    	"color_scheme": "Packages/Theme - Spacegray/base16-ocean.dark.tmTheme",
    	"draw_white_space": "all",
    	"enable_tab_scrolling": false,
    	"fold_buttons": true,
    	"fade_fold_buttons": false,
    	"font_face": "Source Code Pro",
    	"font_size": 17,
    	"highlight_line": true,
    	"highlight_modified_tabs": true,
    	"ignored_packages":
    	[
    		"Vintage"
    	],
    	"line_padding_bottom": 1,
    	"line_padding_top": 1,
    	"show_encoding": true,
    	"show_line_endings": true,
    	"spacegray_sidebar_font_xlarge": true,
    	"spacegray_sidebar_tree_large": true,
    	"spacegray_tabs_auto_width": true,
    	"spacegray_tabs_font_large": true,
    	"spacegray_tabs_large": true,
    	"theme": "Spacegray.sublime-theme",
    	"trim_trailing_white_space_on_save": true,
    	"word_wrap": "auto"
    }




注：代码设置均在 `Preferences` > `Settings - User` 中写入。

###1. Theme - Spacegray

三种风格可选：

**Spacegray** 

默认版本基于 Base16 Ocean Dark 配色方案。

![](http://cdn.saymagic.cn/o_19qj707jk13d714i2dl1ao81c0q9.png)

设置：

    {
      "theme": "Spacegray.sublime-theme",
      "color_scheme": "Packages/Theme - Spacegray/base16-ocean.dark.tmTheme"
    }


**Spacegray Light**

浅色版本基于 Base16 Ocean Light 配色方案。

![](http://cdn.saymagic.cn/o_19qj75iks1rj1ejptsj18pv1hj4e.png)

设置：

    {
      "theme": "Spacegray Light.sublime-theme",
      "color_scheme": "Packages/Theme - Spacegray/base16-ocean.light.tmTheme"
    }

**Spacegray Eighties**

深色版本基于 Base16 Eighties Dark 配色方案。

![](http://cdn.saymagic.cn/o_19qj7b97eeej1dt8c461c0q6ni9.png)

设置：

    {
      "theme": "Spacegray Eighties.sublime-theme",
      "color_scheme": "Packages/Theme - Spacegray/base16-eighties.dark.tmTheme"
    }


----------

**其他附加设置：**

选项卡字体可设：


    "spacegray_tabs_font_small": true

>     "spacegray_tabs_font_normal": true

 <pre class="lang-css">"spacegray_tabs_font_large": true</pre>
 <pre>"spacegray_tabs_font_xlarge": true</pre>

选项卡高度可设：

>     "spacegray_tabs_small": true

    "spacegray_tabs_normal": true
>     
>     "spacegray_tabs_large": true

    "spacegray_tabs_xlarge": true

选项卡宽度可设：

>     "spacegray_tabs_auto_width": true

侧边栏标签字体大小：

    "spacegray_sidebar_font_small": true
>     "spacegray_sidebar_font_normal": true

    "spacegray_sidebar_font_large": true

>     "spacegray_sidebar_font_xlarge": true

侧边栏文件树行高：

    "spacegray_sidebar_tree_xsmall": true

>     "spacegray_sidebar_tree_small": true

    "spacegray_sidebar_tree_normal": true

>     "spacegray_sidebar_tree_large": true

    "spacegray_sidebar_tree_xlarge": true

Sublime Text3 中隐藏导航图标：

    "enable_tab_scrolling": false

我的附加设置：

    {
     	"spacegray_tabs_font_large": true,
    	"spacegray_tabs_large": true,
    	"spacegray_tabs_auto_width": true,
    	"spacegray_sidebar_font_xlarge": true,
    	"spacegray_sidebar_tree_large": true, 
    	"enable_tab_scrolling": false
    }
    
###2. BracketHighlighter

这个插件的高亮自定义标签需要我们根据自己的配色方案和个人喜好来设置。这里基于 Base16 Ocean Dark 配色方案的 Spacegray 主题。

`Preferences` > `Package Settings` >  `Bracket Highlighter` > `Bracket Settings - User` 中写入以下内容：

    {
	    "bracket_styles": {
		    "default": {
		    "icon": "dot",
		    "color": "brackethighlighter.default",
		    "style": "outline"
		    },
		    "unmatched": {
		    "icon": "question",
		    "color": "brackethighlighter.unmatched",
		    "style": "outline"
		    },
		    "curly": {
		    "icon": "curly_bracket",
		    "color": "brackethighlighter.curly",
		    "style": "outline"
		    },
		    "round": {
		    "icon": "round_bracket",
		    "color": "brackethighlighter.round",
		    "style": "outline"
		    },
		    "square": {
		    "icon": "square_bracket",
		    "color": "brackethighlighter.square",
		    "style": "underline"
		    },
		    "angle": {
		    "icon": "angle_bracket",
		    "color": "brackethighlighter.angle",
		    "style": "underline"
		    },
		    "tag": {
		    "icon": "tag",
		    "color": "brackethighlighter.tag",
		    "style": "highlight"
		    },
		    "c_define": {
		    "icon": "hash"
		    },
		    "single_quote": {
		    "icon": "single_quote",
		    "color": "brackethighlighter.single_quote",
		    "style": "outline"
		    },
		    "double_quote": {
		    "icon": "double_quote",
		    "color": "brackethighlighter.double_quote",
		    "style": "outline"
		    },
		    "regex": {
		    "icon": "regex"
		    }
	    }
    }

对应关系：

    {} － curly
    () － round
    [] － square
    <> － angle
    “” ” － quote

style: solid、underline、outline、highlight

在安装目录中找到 Packages 文件夹（如果你使用的是默认主题） 中对应的主题文件，或者我的是`\Sublime Text 3\Data\Installed Packages`中的 `Theme - Spacegray.sublime-package`。
把 `Theme - Spacegray.sublime-package` 加上后缀`.zip`，解压，找到需要更改的配色方案文件。我的是 `base16-ocean.dark.tmTheme` 。编辑这个文件，在 `<array></array>` 中追加这样的内容：
    
    		<dict>
    			<key>name</key>
    			<string>Bracket Default</string>
    			<key>scope</key>
    			<string>brackethighlighter.default</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#B48EAD</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Unmatched</string>
    			<key>scope</key>
    			<string>brackethighlighter.unmatched</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#FAC169</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Curly</string>
    			<key>scope</key>
    			<string>brackethighlighter.curly</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#BF616A</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Round</string>
    			<key>scope</key>
    			<string>brackethighlighter.round</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#EBCB8B</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Square</string>
    			<key>scope</key>
    			<string>brackethighlighter.square</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#A3BE8C</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Angle</string>
    			<key>scope</key>
    			<string>brackethighlighter.angle</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#D08770</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Tag</string>
    			<key>scope</key>
    			<string>brackethighlighter.tag</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#EFF1F5</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Singel Quote</string>
    			<key>scope</key>
    			<string>brackethighlighter.single_quote</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#96B5B4</string>
    			</dict>
    		</dict>
    		<dict>
    			<key>name</key>
    			<string>Bracket Double Quote</string>
    			<key>scope</key>
    			<string>brackethighlighter.double_quote</string>
    			<key>settings</key>
    			<dict>
    				<key>foreground</key>
    				<string>#8FA1B3</string>
    			</dict>
    		</dict>

完成后保存，把压缩包恢复原先的样子。重启 Sublime 可看到效果。高亮的符号有颜色啦。
 
*注：更改主题包时， Sublime 需处于关闭状态。*






