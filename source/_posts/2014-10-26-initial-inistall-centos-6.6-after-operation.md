---
layout: post
title: "初始化安装 CentOS 6.6 后简单设置操作"
date: 2014-10-26 18:49:44 +0800
comments: true
categories: [运维]
tags: [centos,linux]
---

从 08 年一接触 Linux 系统就用的 Red Hat 系列的操作系统.也一直都在使用这个系列的。Debian系的也算用的多的，但还是 Red Hat系的比较熟悉。实在无聊试试Markdown的代码格式化，随便写写玩。

本文是在刚刚安装好的 CentOS 6.6 的一次简单操作设置记录。

<!--more-->

## 一、安装系统 ##

使用 `CentOS-6.6-x86_64-minimal.iso` 安装全新操作系统.

...略

- 语言：英文
- 时区：上海（东八区）

...略

## 二、基本操作 ##


- 激活网卡、设置DNS

``` sh
# ifup eth1
# echo 'nameserver 8.8.8.8' >> /etc/resolv.conf
# echo 'nameserver 114.114.114.114' >> /etc/resolv.conf
```

- 设置网卡开启启动

编辑对应网卡配置文件中 `ONBOOT=no` 为 `ONBOOT=yes`

```
# vi /etc/sysconfig/network-scripts/ifcfg-eth1
```

- 更新系统

``` sh
# yum update -y
```

### 安装几个常用工具 ###

``` bash
$ sudo yum install vim -y
$ sudo yum install wget -y
$ sudo yum install unzip -y
$ sudo yum install git-all -y
$ sudo yum -y install gcc gcc-c++
```
