---
layout: post
title: "Install jekyll"
date: 2015-02-13 12:21:00 +0800
comments: true
categories: 
---

一、安装系统
--------------------------
使用`CentOS-6.6-x86_64-minimal.iso`安装全新操作系统

- 语言：英文
- 时区：上海（东八区）

二、安装操作系统后系统初始化设置
--------------------------------

- 激活网卡、设置DNS
	- 激活网卡
    
	```
	#ifup eth1
	```

	- 修改DNS
    
	```
	#echo 'nameserver 8.8.8.8' >> /etc/resolv.conf
	#echo 'nameserver 114.114.114.114' >> /etc/resolv.conf
	```

	- 设置网卡开启启动
    
	```
	#vi /etc/sysconfig/network-scripts/ifcfg-eth1
	```
	编辑`ONBOOT=no`为`ONBOOT=yes`
	
- 更新系统

```
#yum update -y
```

- 安装工具环境

```
#yum install vim -y
#yum install wget -y
#yum install unzip -y
#yum install git-all -y
```

三、编译安装Ruby 1.9.3
--------------------------

http://www.cnblogs.com/iosdev/p/3320671.html

1. 准备需要的安装的东西

```
yum -y install make gcc openssl-devel zlib-devel gcc gcc-c++ make autoconf readline-devel curl-devel expat-devel gettext-devel ncurses-devel sqlite3-devel mysql-devel httpd-devel wget which
```

2. 下载源文件

```
cd /usr/src
wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz
tar zxf yaml-0.1.4.tar.gz
cd yaml-0.1.4
./configure --prefix=/usr/local
make && make install
```

``` 
cd /usr/src
wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p551.tar.gz
tar zxf ruby-1.9.3-p551.tar.gz
cd ruby-1.9.3-p551
./configure --prefix=/usr/local --disable-install-doc --with-opt-dir=/usr/local/lib
make && make install
```
<!--
	- 编译Ruby的时候遇到错误
    
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
    
	解决办法参照如下网页
	http://blog.csdn.net/iefreer/article/details/18828515
-->
3. whereis 查看ruby路径

``` sh
whereis ruby
```

三、安装jekyll
-----------------------------

~~#yum install ruby rubygemc~~不知道为什么执~行完了没有装上~

~~#yum install ruby-rdo~~    
~~#yum install ruby-devel~~
~~#yum install gcc gcc-c++~~


```
#wget http://production.cf.rubygems.org/rubygems/rubygems-2.4.5.tgz
#tar -vxf rubygems-2.4.5.tgz
#cd rubygems-2.4.5
#ruby setup.rb
```
```
gem install jekyll

gem install RedCloth #这个为了解决运行jekyll的时候出错的问题
gem install rdiscount
```

``` sh
cd blog/
jekyll server -H 0.0.0.0
```
浏览器访问 http://127.0.0.1:4000


- PS无法访问https://api.rubygem.org可以参照https://ruby.taobao.org替换使用淘宝的源

    替换淘宝源后安装有些包出错,最终还是切换回官方源安装成功
    
- 需要先安装nodejs


四、安装Raneto
-------------------------

``` bash
wget http://nodejs.org/dist/v0.10.35/node-v0.10.35-linux-x64.tar.gz
tar zvxf node-v0.10.35-linux-x64.tar.gz
mv node-v0.10.35-linux-x64 ~/opt/nodejs
ln -s ~/opt/nodejs/bin/node ~/bin/node
ln -s ~/opt/nodejs/bin/npm ~/bin/npm
```
``` bash
wget https://github.com/gilbitron/Raneto/archive/0.6.0.zip Raneto-0.6.0.zip
unzip Raneto-0.6.0.zip
mv Raneto-0.6.0 ~/www/raneto

cd ~/www/raneto
npm install
npm start
```
浏览器访问 http://127.0.0.1:3000/

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using GitCafeSDK;
using System.Net;

namespace Simple
{
    class Program
    {
        static void Main(string[] args)
        {

        }
    }
}
```
