---
layout: post
title: "Git常用难记命令"
date: 2015-02-28 14:19:34 +0800
comments: true
categories: "git"
tags:  ["git","命令行","手册"]
---

{% blockquote @shepherdlazy,twitter http://www.twitter.com/shepherdlazy %}
在日常使用git中经常会使用到一些非常有用的命令脚本，但是我的脑袋总是记不住，每次都要上网查询，故在这里做一个集锦。
{% endblockquote %}

<!--more-->

- #### 创建一个空分支
	``` bash
	$ git checkout --orphan gh-pages
	```

- #### 删除远程分支
- #### 恢复版本库到上一个版本
- #### 重置本地文件
- #### 强制推送
- #### 创建、删除、签出分支
- #### 同步全部远程分支
- #### 克隆全部远程分支
- #### 拉取全部远程分支
- #### 以子目录形式引入外部项目
	``` bash
	$ git filter-branch
	$ git subtree
	```
	- http://www.tuicool.com/articles/veaEBr
- #### 拆分仓库
	- http://www.tuicool.com/articles/rYBVzmy
	- http://codego.net/153650/
	- http://codego.net/21462/

- #### 如何提交空目录
	为什么不能提交空目录呢？
```
$ find . ( -type d -empty ) -and ( -not -regex ./\.git.* ) -exec touch {}/.gitignore;
```		
	- http://demo.netfoucs.com/younger_china/article/details/21653463

- #### 下一个
