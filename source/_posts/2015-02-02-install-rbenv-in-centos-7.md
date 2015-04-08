---
layout: post
title: "在 CentOS 7 上安装 rbenv"
date: 2015-02-02 08:43:08 +0800
updated: 2015-04-04 12:43:08 +0800
comments: true
categories: "环境安装"
tags: [rbenv,ruby,linux,centos]
---

rbenv 用来管理多个版本的 ruby 在用户目录的安装和使用.也可以使用 rvm 来实现多个版本的 ruby 安装和使用.

<!--more-->

## 安装 rbenv ##

``` bash
[dukelazy@localhost ~]$ cd ~
[dukelazy@localhost ~]$ git clone git://github.com/sstephenson/rbenv.git .rbenv
[dukelazy@localhost ~]$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
[dukelazy@localhost ~]$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
[dukelazy@localhost ~]$ git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
[dukelazy@localhost ~]$ source ~/.bash_profile
```

## 安装 Ruby 1.9.3 ##

```
[dukelazy@localhost ~]$ rbenv install 1.9.3-p551
[dukelazy@localhost ~]$ rbenv local 1.9.3-p551
[dukelazy@localhost ~]$ rbenv rehash
[dukelazy@localhost ~]$ rm -rf .rbenv-version
```

## *参考文档* ##

- https://github.com/sstephenson/rbenv [2015-02-03].
- ftp://ftp.ruby-lang.org/pub/ruby/ [2015-02-03].
- http://cache.ruby-lang.org/pub/ruby/1.9/ [2015-02-03].
- https://ruby-china.org/wiki/rbenv-guide [2015-02-03].
- http://octopress.org/docs/setup/rbenv/ [2015-02-03].

