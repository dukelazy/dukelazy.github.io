---
layout: post
#title: "Explain the folder tree structure important MFS system"
title: "重要文件夹结构说明"
date: 2015-02-26 14:53:28 +0800
comments: true
categories: 
---

```
/                   #根文件系统
+---archive
+---backup
|   \---website
+---book
|   |   1.rtf
|   |   q.txt
|   |   sf.txt
|   |
|   +---fd
|   \---fs
+---current
+---document
+---Environment
+---list
+---Resources
+---software
\---workspace
```

- Octopress重点关注的文件夹和文件
```
octopress.
|   config.rb
|   config.ru
|   Gemfile
|   Rakefile
|   _config.yml
|   
+---.themes
+---plugins
+---public  
+---source
\---_deploy
```

- Raven-GitCafePC上的重要文件夹以及文件
```
Raven-GitCafePC.
+---C:
|   +---SRE
|   \---Users
|       \---Raven
|           |    .bash_history
|           |    .gemrc
|           |    .gitconfig
|           |    .gvimrc
|           |    .npmrc
|           |    .viminfo
|           |    .vimrc
|           |    _viminfo
|           |
|           +---.gnupg
|           +---.ssh
|           \---.vim
|
+---D:
|   +---KuGou
|   +---Resources
|   \---SoftWare
|
+---E
|   +---dev
|   +---scm
|   +---Sync
|   \---Tools
|
\---F:
    \---PT
```