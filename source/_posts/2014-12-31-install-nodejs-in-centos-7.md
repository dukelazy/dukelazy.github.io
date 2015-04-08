---
layout: post
title: "在 CentOS 7 上安装 Node.js"
date: 2014-12-31 ‏‎12:20:56 +0800
updated: 2015-04-04 17:36:55 +0800
comments: true
categories: [环境安装]
tags: [node.js,centos,linux]
---

Node.Js 是一个 Javascript 运行环境 (runtime),无论在 Windows 还是 Linux 上安装和使用都很方便.现在有很多程序都是使用 Node.Js 作为运行框架, Node.Js 也有非常丰富的扩展库.下面主要记录我在 Centos 7 上的安装过程.使用两种不同的方式安装,我要使用最新的稳定版本所以使用第二种方法安装的.

<!--more-->

## 系统信息 ##

``` bash
[dukelazy@localhost ~]$ cat /etc/redhat-release
CentOS Linux release 7.0.1406 (Core)
[dukelazy@localhost ~]$ uname -r
3.10.0-123.20.1.el7.x86_64
[dukelazy@localhost ~]$ python --version
Python 2.7.5
```

## 安装方法一 ##

使用yum安装.适用于有管理员权限的用户，安装后整个系统全部用户均可用。

``` bash
[root@localhost ~]# curl -sL https://rpm.nodesource.com/setup | bash -
[root@localhost ~]# yum install -y gcc-c++ make zlib-devel openssl-devel
[root@localhost ~]# yum install -y nodejs
```

查看安装的版本

``` bash
[dukelazy@localhost ~]$ node --version
v0.10.38
[dukelazy@localhost ~]$ npm --version
1.4.28
```

## 安装方法二 ##

适用于没有管理员权限或者想安装不同版本等情况.
在[Node.js](https://nodejs.org/download/ "Node.js")官网查看符合自己系统版本的包下载解压到安装目录,配置环境变量.

```
[dukelazy@localhost ~]$ wget http://nodejs.org/dist/v0.12.2/node-v0.12.2-linux-x64.tar.gz
[dukelazy@localhost ~]$ tar zvxf node-v0.12.2-linux-x64.tar.gz
[dukelazy@localhost ~]$ mkdir -p $HOME/opt
[dukelazy@localhost ~]$ mv node-v0.12.2-linux-x64 $HOME/opt/
[dukelazy@localhost ~]$ mkdir -p $HOME/bin
[dukelazy@localhost ~]$ ln -s $HOME/opt/node-v0.12.2-linux-x64/bin/node $HOME/bin/
[dukelazy@localhost ~]$ ln -s $HOME/opt/node-v0.12.2-linux-x64/bin/npm $HOME/bin/
[dukelazy@localhost ~]$ source $HOME/.bash_profile
[dukelazy@localhost ~]$ rm -rf node-v0.12.2-linux-x64.tar.gz
```

查看安装的版本

```
[dukelazy@localhost ~]$ node --version
v0.12.2
[dukelazy@localhost ~]$ npm --version
2.7.4
```

## *参考资料* ##

- *[https://nodejs.org/](https://nodejs.org "Node.js"). [2014-12-31].*
- *[Node.js in github](https://github.com/joyent/node "Node.js Git Repository"). [2014-12-31].*
- *[Installing Node.js via package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager "Installing Node.js via package manager"). [2014-12-31].*

