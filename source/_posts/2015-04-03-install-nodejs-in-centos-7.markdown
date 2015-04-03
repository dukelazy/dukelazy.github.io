---
layout: post
title: "install nodejs in CentOS 7"
date: 2015-04-03 08:04:55 +0800
comments: true
categories: 
---
### 我的操作系统环境信息 ###
``` bash
[dukelazy@localhost ~]$ cat /etc/redhat-release 
CentOS Linux release 7.0.1406 (Core) 
[dukelazy@localhost ~]$ uname -r
3.10.0-123.20.1.el7.x86_64
[dukelazy@localhost ~]$ python --version
Python 2.7.5
```

### 安装方法一 ###

适用于有管理员权限的用户，安装后整个系统全部用户均可用。
``` bash
[root@localhost ~]# curl -sL https://rpm.nodesource.com/setup | bash -
[root@localhost ~]# yum install -y nodejs
[root@localhost ~]# yum install -y gcc-c++ make
```

``` bash
[dukelazy@localhost ~]$ node --version
v0.10.38
[dukelazy@localhost ~]$ npm --version
1.4.28
```

### 安装方法二 ###

适用于没有管理员权限以及想安装不同版本等情况。
在[Node.js](https://nodejs.org/download/ "Node.js")官网查看符合自己系统版本的包下载并安装
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

```
[dukelazy@localhost ~]$ node --version
v0.12.2
[dukelazy@localhost ~]$ npm --version
2.7.4
```
### 参考文档 ###
- *[作者名称](个人主页URL "作者名称"). [文章标题](文章URL "文章标题"). [网站](网站首页url "网站名称"). 发布日期 [引用日期].*
- [https://nodejs.org/](https://nodejs.org "Node.js").
- [Node.js in github](https://github.com/joyent/node "Node.js Git Repository")
- [Installing Node.js via package manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager "Installing Node.js via package manager")

