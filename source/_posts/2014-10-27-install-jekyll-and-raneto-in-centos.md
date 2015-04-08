---
layout: post
title: "在 CentOS 上安装 Jekyll 与 Raneto"
date: 2014-10-27 ‏‎12:20:56 +0800
updated: 2015-04-04 19:24:55 +0800
comments: true
categories: [环境安装]
tags: [jekyll,blog,raneto,centos,linux]
---

{% blockquote 我, 回头看过 %}
一天竟瞎折腾。
{% endblockquote %}


准备构建知识管理系统，安装两个使用 Markdown 语法的静态网页生成程序，先安装上试试，不知道到底咋样。

<!--more-->

## 一、安装准备 ##

```
# wget http://production.cf.rubygems.org/rubygems/rubygems-2.4.5.tgz
# tar -vxf rubygems-2.4.5.tgz
# cd rubygems-2.4.5
# ruby setup.rb
# gem install RedCloth #这个为了解决运行jekyll的时候出错的问题
# gem install rdiscount
```

- **PS:**
- 使用gem安装的时候，如果无法访问https://api.rubygem.org可以参照https://ruby.taobao.org替换使用淘宝的源。
- 我去这次替换淘宝源后安装有些包出错,最终还是切换回官方源安装成功。

## 二、安装jekyll ##

```
# gem install jekyll
```

## 三、使用 Jekyll ##

```
# cd ~
# jekyll install blog
# cd blog/
# jekyll server -H 0.0.0.0
```

浏览器访问 http://127.0.0.1:4000

## 四、安装Raneto ##

- 首先安装Node.Js

```
wget http://nodejs.org/dist/v0.10.35/node-v0.10.35-linux-x64.tar.gz
tar zvxf node-v0.10.35-linux-x64.tar.gz
mv node-v0.10.35-linux-x64 ~/opt/nodejs
ln -s ~/opt/nodejs/bin/node ~/bin/node
ln -s ~/opt/nodejs/bin/npm ~/bin/npm
```

- 下载安装Raneto

```
wget https://github.com/gilbitron/Raneto/archive/0.6.0.zip Raneto-0.6.0.zip
unzip Raneto-0.6.0.zip
mv Raneto-0.6.0 ~/www/raneto

cd ~/www/raneto
npm install
npm start
```

浏览器访问 http://127.0.0.1:3000/
