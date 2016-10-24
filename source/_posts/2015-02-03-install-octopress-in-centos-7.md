---
layout: post
title: "在 CentOS 7 上安装 Octopress"
date: 2015-02-03 08:42:43 +0800
updated: 2015-04-04 17:40:55 +0800
comments: true
categories: "环境安装"
tags: ["octopress","blog","ruby","centos"]
---

在一个全新的系统上,重新安装了 Octopress 记录下安装过程.在此之前要先安装 Octopress 的运行环境 Ruby 1.9.3 与 Node.Js

<!--more-->

## 安装过程 ##
``` bash
[dukelazy@localhost ~]$ cd ~
[dukelazy@localhost ~]$ git clone https://github.com/imathis/octopress.git octopress

[dukelazy@localhost octopress]$ cd octopress
[dukelazy@localhost octopress]$ rbenv local 1.9.3-p551
[dukelazy@localhost octopress]$ rbenv rehash

[dukelazy@localhost octopress]$ gem install bundler
[dukelazy@localhost octopress]$ rbenv rehash

[dukelazy@localhost octopress]$ bundle install -V # 可以编辑Gemfile，使用淘宝的源
```

``` bash
[dukelazy@localhost octopress]$ rake install
mkdir -p source
cp -r .themes/classic/source/. source
mkdir -p sass
cp -r .themes/classic/sass/. sass
mkdir -p source/_posts
mkdir -p public
```

## 错误处理 ##

1. 在安装 Ruby 之前没有安装 zlib-devel,安装 zlib-devel 后重新安装 Ruby 即可.

    - 错误信息如下:

    ```
    [dukelazy@localhost octopress]$ gem install bundler
    ERROR:  Loading command: install (LoadError)
        cannot load such file -- zlib
    ERROR:  While executing gem ... (NameError)
        uninitialized constant Gem::Commands::InstallCommand
    ```

    - 处理过程:

    ```
    [dukelazy@localhost octopress]$ sudo yum install zlib-devel
    [dukelazy@localhost octopress]$ rbenv uninstall 1.9.3-p551
    [dukelazy@localhost octopress]$ rbenv install 1.9.3-p551
    [dukelazy@localhost octopress]$ rbenv rehash
    ```

    Ubuntu 14.04.5 LTS
    
    ``` bash
    apt-get install zlib1g
    apt-get install zlib1g-dev
    ```

2. 在安装 Ruby 之前没有安装 openssl-devel,安装 openssl-devel 后重新安装 Ruby 即可.

    - 错误信息如下:

    ``` bash
    [dukelazy@localhost octopress]$ bundle install
    Could not load OpenSSL.
    You must recompile Ruby with OpenSSL support or change the sources in your
    Gemfile from 'https' to 'http'. Instructions for compiling with OpenSSL using
    RVM are available at rvm.io/packages/openssl.
    ```

    - 处理过程:

    ``` bash
    [dukelazy@localhost octopress]$ sudo yum install openssl-devel
    [dukelazy@localhost octopress]$ rbenv install 1.9.3-p551
    [dukelazy@localhost octopress]$ rbenv rehash
    ```

    Ubuntu 14.04.5 LTS

    ``` bash
    apt-get install openssl
    apt-get install libssl-dev
    ```
