---
layout: post
title: "MFS directory Tree planning"
date: 2015-04-04 22:39:24 +0800
comments: true
categories: 
---

为不同文件夹根据重要级别,可再生级别,保密级别配置不同的备份和共享访问策略

```
/ mfs                   #根文件系统,[环境,程序,数据]
+---scm     #GitRepository   svnRepository
+---src     #源代码,没有纳入版本库的,还有第三方下载的,重要版本的发布版源码
+---archive #dist,软件程序发布包
+---doc     #document,文档,图书等
+---media   #对媒体资料
|   +---music   #音乐资料
|   +---videos  #视频资料
|   +---recorder#录音资料
|   \---gallery #相册
+---backup  #重要的备份,数据库备份,服务器配置,运行环境配置等,操作系统等非常大的备份,不打算放在这里
+---tools   #这里包含mfs管理脚本,如果只放在mfs中当mfs没有挂载的时候会无法使用
\---www # 网站,这个主要是为了提高小文件读取速度,如果配置好备份等,可以从scm与backup等地快速重建
#虚拟目录
share   #共享目录-samba
nfs     #NFS共享
ftp     #ftp读写目录-ftp
pub     #共享目录-http
ext     #扩展池挂载目录


可以重建的(包括可以从网络下载的)用mfspool-ext,这个池暂时不作冗余备份
+---software
+---SRE     #软件运行环境(software run environment)
+---Users
|   +---dukelazy
|   +---shepherd
|   \---Raven
+---machine #不同的计算机或操作系统
\---workspace   #工作目录,主要指开发和文档编写
+---movie
+---current



+---list
+---Resources
```

rsync-to-zfs

```
#! /bin/bash
#rsync -avP /mfs/pub/mnt/sdc1 /mfs/pub/mnt/sdb6/
#rsync -avP /mfs/pub/mnt/sdc2 /mfs/pub/mnt/sdb6/
#rsync -avP /mfs/pub/mnt/sdd3 /mfs/pub/mnt/sdb6/

#rsync -avP /mfs/pub/mnt/sde3 /mfs/pub/mnt/sdb6/
#rsync -avP /mfs/pub/mnt/sde1 /mfs/pub/mnt/sdb6/

rsync -avP /mfs/pub/mnt/sdb6/sde1/KuGou /mfspool/media/music
rsync -avP /mfs/pub/mnt/sdb6/sdc1/galleries   /mfspool/media/gallery

rsync -avP /mfs/pub/mnt/sdb6/sdd3/SoftWare/ /mfspool-ext/software/gitcafe-software/

rsync -avP /mfs/pub/mnt/sdb6/sdc1/行车记录仪-过年 /mfspool/media/videos
rsync -avP /mfs/pub/mnt/sdb6/sdc1/结婚照光盘镜像 /mfspool/media/gallery

rsync -avP /mfs/pub/mnt/sdb6/sdc2/server1.myrsoft.net /mfspool/backup
rsync -avP /mfs/pub/mnt/sdb6/sdc2/yanyu /mfspool/backup


rsync -avP /mfs/pub/mnt/sdb6/sde1/电影 /mfspool-ext/movie
rsync -avP /mfs/pub/mnt/sdb6/sde3/happycasts.net /mfspool-ext/movie
rsync -avP /mfs/pub/mnt/sdb6/sde3/wpf教程  /mfspool-ext/movie
rsync -avP /mfs/pub/mnt/sdb6/transmission /mfspool-ext/movie

rsync -avP /mfs/pub/mnt/sdb6/sdd3/迅雷下载 /mfspool-ext/software
```
