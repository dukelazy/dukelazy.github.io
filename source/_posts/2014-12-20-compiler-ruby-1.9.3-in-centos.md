---
layout: post
title: "在 CentOS 上编译安装 ruby 1.9.3 "
date: 2014-12-20 18:16:01 +0800
updated: 2015-04-04 17:36:55 +0800
comments: true
categories: [环境安装]
tags: [ruby,centos,linux]
---

以下命令都是在超级管理员权限下执行，如果不是超级管理员，请自行调整安装目录和命令。

<!--more-->

## 准备编译环境 ##

``` bash
sudo yum -y install make gcc openssl-devel zlib-devel gcc gcc-c++ make autoconf readline-devel curl-devel expat-devel gettext-devel ncurses-devel sqlite3-devel mysql-devel httpd-devel wget which
```

## 编译 yaml 支持 ##

``` bash
cd /usr/src
wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz
tar zxf yaml-0.1.4.tar.gz
cd yaml-0.1.4
./configure --prefix=/usr/local
make && make install
```

## 编译 Ruby ##

``` bash
cd /usr/src
wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p551.tar.gz
tar zxf ruby-1.9.3-p551.tar.gz
cd ruby-1.9.3-p551
./configure --prefix=/usr/local --disable-install-doc --with-opt-dir=/usr/local/lib
make && make install
```

## 检查安装 ##

``` bash
whereis ruby
ruby --version
```

## 编译错误处理 ##

- 编译Ruby的时候遇到如下错误，解决办法参照网页 http://blog.csdn.net/iefreer/article/details/18828515

```
ossl_pkey_ec.c:765: warning: assignment makes pointer from integer without a cast
ossl_pkey_ec.c:819: error: ‘EC_GROUP_new_curve_GF2m’ undeclared (first use in this function)
ossl_pkey_ec.c:819: error: (Each undeclared identifier is reported only once
ossl_pkey_ec.c:819: error: for each function it appears in.)
ossl_pkey_ec.c: In function ‘ossl_ec_group_set_seed’:
ossl_pkey_ec.c:1114: warning: comparison between signed and unsigned integer expressions
make[1]: *** [ossl_pkey_ec.o] Error 1
make[1]: Leaving directory `/usr/src/ruby-1.9.2-p330/ext/openssl'
make: *** [mkmain.sh] Error 1
```
    

    




## *参考资料* ##

- http://www.cnblogs.com/iosdev/p/3320671.html
- http://blog.csdn.net/iefreer/article/details/18828515
