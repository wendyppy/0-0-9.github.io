---
layout: post
title: "git常用命令总结"
keywords: ["git ",".DS_Store"]
description: "git的常用命令"
category: "Git"
tags: ["Git"]
---
{% include JB/setup %}

### 配置账号信息

	git config --global user.name ***
	git config --global user.email ***@***.***
	git config --list# 查看配置的信息
	git help config# 获取帮助信息
	

### 生成密钥

	ssh-keygen -t rsa -C ***@***.***

###  初始化 

**初始化一个目录为工作目录**

    git init

###  向临时仓库添加文件 

**'  .   '表示全部添加，当你需要添加刚才所修改的所有文件时，就可用点表示。当然，你也可以添加具体某一文件，如git add a.doc**

    git add .   

###  向永久仓库提交文件

**add 命令是向git的临时仓库提交，临时仓库类似缓冲区，commit 表示真正的向代码库提交。**

    git commit . -m "注释部分"

###  创建并切换分支 

**建立分支是创建代码的独立版本的动作，独立于主干分支[主分支(名称叫做master)，即是我们干刚提交的分支，当你没有指定时，git会默认帮我们提交到主分支上。主分支是默认生成的，其它分支需要自己创建]。

创建并同时切换到你新建的分支，发送：**

    git checkout -b new_feature

**上面的命令我们可以分两步执行，先创建一个分支然后手动切换，就像这样：**

    创建新分支：git branch new_featuregit
	切换到另一分支: checkout new_feature

**如果要看在项目下所有的分支，发送这个：**

	git branch

**现在你可以在你的项目上无所顾忌地做任何你想做的：任何时候，你都可以回到你创建分支前的状态。**

###  合并分支 

**当我们对分支上的新功能满意了的时候，就需要把它加到主干分支上。当我们在自己的新功能分支上时，首先需要加载（stage）并且提交你的文件：**

    git add .t commit -m "adds my new feature"

**然后我们移到自己的主干分支：**

    git checkout master

**像这样合并：**

	git merge new_feature

**此时，你的主干分支和你的新功能分支会变成一样的。**

###  版本回退

**git会帮你记录你的任何一次提交（所以，提交时写注释是很需要的，方便我们找回以前版本），何时切换了哪些分支等等。查看的命令很简单：**

	git reflog

**你会发现git是用一些不规律的16进制数代表一个版本，我们如果想回到某一个以前的版本，可以用下面的命令：**

	git reset --hard 该版本的16进制数
	(注：只需写前几位即可，git会自己去寻找) 

	

**如果不是特殊情况，我们应该很少去回退到特别古老的版本，最常用的莫过于对自己刚刚做的修改不满意，（在上一次commit之后做了修改，之后还没有做add和commit操作）想撤回到上一次的commit。你只需写一个命令即可回退到刚才commit的状态:**

    git checkout -- . 
	(特别注意：checkout后面是两杠-- 如果没有这两杠是什么？
	就是切换分支了！要特别注意！当然，--后面还需要 '  .  '，
	表示全部撤回。)
	

### 标签

	 
 
	git tag v1.0# 新建标签
	git tag -a v1.0 -m 'my version 1.0'# 新建带注释标签
 
	git checkout tagname# 切换到标签
	git tag# 列出现有标签 
 
	push origin v1.5# 推送分支到源上
	git push origin --tags# 一次性推送所有分支
 
	git tag -d v1.0# 删除标签
	git push origin :refs/tags/v1.0# 删除远程标签
	

###  上传文件到远程代码库

**之所以说git 强大，是因为我们刚刚所有的操作都是在本地完成的，不需要联网，不需要自己额外建库，这是svn，cvs等其它版本管理工具做不到的。但这样岂不是不能团队合作了？别急，这就有了github，bitbucket等网站，它们为开发者免费提供远程代码库。我们可以很方便的将自己的代码存储到上面，也可以很方便的将远程库的代码pull到本地，实现团队开发。**

**本地库与远程库进行绑定：**

	git remote add origin git@server-name:path/repo-name.git 

**接着我们就向这个远程仓库提交代码：**

	git push origin 本地分支名称:远程分支名称 
	(应该很好理解，push代表上传命令，‘   ： ’左边写上本地的需要上传的分支名称，右边写上将这个分支上传到服务的分支名称，)

	git push origin master
	(将本地的master分支推送到远程master分支上，是git push origin master:master的简写形式)

**在团队开发的时候，当我们需要修改一个文件，为了不必要的麻烦，一定记得要将自己的本地文件更新到远程代码库的最新版本后再做修改。更新远程代码库很简单.**

    git pull origin master
    
    

### 使用.gitignore文件

**在一个项目中，往往有些文件是无需纳入 Git 管理的，比如自动生成的文件，像日志或者编译过程中创建的文件。我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件来解决这个问题。**

注意：

1.以`# `符号开始的行都会被git忽视

2.支持`glob模式`匹配（即简化了的正则表达式）

示例如下：


	#  忽略所有 .tmp 结尾的文件
	*.tmp

	#  但 main.tmp 除外
	!main.tmp

	#  仅仅忽略项目根目录下的 note文件
	/note

	#  忽略 build/ 目录下的所有文件
	build/

	#  会忽略 doc/notes.txt 但不包括 doc/server/notes.txt
	doc/notes.txt


在Mac系统中，有一个很讨厌的文件叫做 `.DS_Store`,如果我们使用Mac系统一般会在项目初始化的时候将其加入`.gitignore`，但有时候会因为从Windows迁移等原因导致忘记添加并且commit进仓库了。这时候我们需要将其移出Git仓库，步骤如下:

	find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch

然后再将`.gitignore`里添加`.DS_Store`,重新commit即可。

