---
layout: post
title: "rsync script"
date: 2015-03-30 17:21:47 +0800
comments: true
categories: ["linux","rsync","server"]
---

**注意原路径末尾有没有`/`符号**

- 如果没有`/`则在目录中创建原路径最后的目录;
- 如果有`/`,则在目标路径的根目录下创建源目录中的文件;

<!--more-->
1. **在Windows客户端安装Rsync；**
2. **在Rsync的安装目录下建立`home/.ssh/config`文件**
3. **准备ssh key；**
4. **在Windows客户端以管理员身份执行同步脚本；**
``` bat
rsync -rltDvzP /cygdrive/f/Media/行车记录仪-过年 dukelazy@192.168.0.10:/mfs/pub/mnt/sdb1/
```
5. **在服务器端执行修改权限脚本；**

``` bat
rsync -avP /mfs/pub/mnt/sde5/MySoftWare/结婚照光盘镜像 /mfs/pub/mnt/sdb1/
```

``` bash
[root@host ~]# find /mfs/pub/mnt/sdb1/结婚照光盘镜像 -type d -exec chmod 755 \{\} \;
[root@host ~]# find /mfs/pub/mnt/sdb1/结婚照光盘镜像 -type f -exec chmod 644 \{\} \;
```
- 在Windows Rsync客户端以管理员身份执行如下脚本同步数据到服务器
``` bat
rsync -avP /mfs/pub/mnt/sde5/galleries /mfs/pub/mnt/sdb1/
```
- 在服务器执行如下脚本设置文件夹和文件权限
``` bash
sudo find /mfs/pub/mnt/sdb1/galleries -type d -exec chmod 755 \{\} \;
sudo find /mfs/pub/mnt/sdb1/galleries -type f -exec chmod 644 \{\} \;
```

gitcafe PC同步到服务器
安装后在安装目录下创建Home/等对应目录，并配置.ssh的配置文件如下
``` 
Host *
    StrictHostKeyChecking no
    UserKnownHostsFile =/dev/null

Host 192.168.0.10
   UserKnownHostsFile =/dev/null
   IdentityFile       =/V/authentication/.ssh/root_rsa
```

```
rsync -avP [src] [dest]
```

启动Octopress
``` bat
E:
cd E:\scm\octopress
set LANG=zh_CN.UTF-8
set LC_ALL=zh_CN.UTF-8
rake generate 
```
