---
layout: post
title: "MFS directory Tree planning"
date: 2015-04-04 22:39:24 +0800
updated: 2015-11-10 18:22:35 +0800
comments: true
categories: 
---

```
/
+---home 
|   +---git 
|   \---mfs 
+---mfs -> /home/mfs 
+---mfspool 
+---mfspool-ext 
```

<!--more-->
注意因为 mfspool-ext 的一些目录设置为挂载到 mfspool 的子目录下，如果先挂载 mfspool-ext 可能会造成 mfspool 无法挂载到 `/mfs` 目录下,这个暂时没有找到可以调整挂载顺序的方法，所以使用的时候要注意观察 `/mfs` 目录下是否有 `ftp` 文件夹. 如果没有可能 mfspool 挂载失败了要手动导出后重新按顺序挂载.

```
/mfs                              # 根文件系统,[环境,程序,数据]
+---scm      -> mfspool/scm       # GitRepository   svnRepository
+---src      -> mfspool/src       # 源代码,没有纳入版本库的,还有第三方下载的,重要版本的发布版源码
+---archive  -> mfspool/archive   # dist,软件程序发布包
+---doc      -> mfspool/doc       # document,文档,图书等
+---media    # 媒体资料
|   +---music -> mfspool/music            # 音乐资料
|   +---videos -> mfspool/videos          # 视频资料
|   +---recorder -> mfspool/recorder      # 录音资料
|   +---movie -> mfspool-ext/movie        # 电影等
|   \---gallery -> mfspool/media/gallery  # 相册
+---software -> mfspool-ext/software   #
+---backup    # 重要的备份,数据库备份,服务器配置,运行环境配置等,操作系统等非常大的备份,不打算放在这里
|   +---SRE -> mfspool/SRE                  # 软件运行环境(software run environment)
|   +---data -> mfspool/machine             # 不同的计算机或操作系统
|   +---secret -> mfspool/secret            # 
|   +---Users-> mfspool/users               # 不同用户的用户目录
|   +---machine -> mfspool-ext/machine          # 不同的计算机或操作系统
|   +---current   -> mfspool-ext/current    # 
|   \---workspace -> mfspool-ext/workspace  # 工作目录,主要指开发和文档编写
+---www      -> mfspool/www        # 网站,这个主要是为了提高小文件读取速度,如果配置好备份等,可以从scm与backup等地快速重建
+---tools    -> mfspool/tools      # 这里包含mfs管理脚本,如果只放在mfs中当mfs没有挂载的时候会无法使用
+---tmp      -> mfspool-ext/tmp    # 
\---pub      -> mfspool/pub        # 公开资料，尽量都是软连接
```

```
# zfs list
NAME                     USED  AVAIL  REFER  MOUNTPOINT
mfspool                  356G  2.29T   173G  /mfspool
mfspool-ext              794G  1.86T   257G  /mfspool-ext
mfspool-ext/current      136K  1.86T   136K  /mfs/backup/current
mfspool-ext/movie        347G  1.86T   347G  /mfs/media/movie
mfspool-ext/software     190G  1.86T   190G  /mfspool-ext/software
mfspool-ext/tmp          136K  1.86T   136K  /mfs/tmp
mfspool-ext/workspace    136K  1.86T   136K  /mfs/backup/workspace
mfspool/archive         13.8G  2.29T  13.8G  /mfs/archive
mfspool/backup          96.9G  2.29T  96.9G  /mfs/backup
mfspool/backup/SRE       136K  2.29T   136K  /mfs/backup/SRE
mfspool/backup/machine   136K  2.29T   136K  /mfs/backup/machine
mfspool/backup/users     136K  2.29T   136K  /mfs/backup/users
mfspool/doc             2.43G  2.29T  2.43G  /mfs/doc
mfspool/media           69.8G  2.29T   176K  /mfs/media
mfspool/media/gallery   39.3G  2.29T  39.3G  /mfs/media/gallery
mfspool/media/music     3.04G  2.29T  3.04G  /mfs/media/music
mfspool/media/recorder   969M  2.29T   969M  /mfs/media/recorder
mfspool/media/videos    26.6G  2.29T  26.6G  /mfs/media/videos
mfspool/pub              136K  2.29T   136K  /mfs/pub
mfspool/scm              168M  2.29T   168M  /mfs/scm
mfspool/src              136K  2.29T   136K  /mfs/src
mfspool/tools            136K  2.29T   136K  /mfs/tools
mfspool/www              136K  2.29T   136K  /mfs/www
```

```
# zpool status
  pool: mfspool
 state: ONLINE
status: Some supported features are not enabled on the pool. The pool can
    still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
    the pool may no longer be accessible by software that does not support
    the features. See zpool-features(5) for details.
  scan: none requested
config:

    NAME                                          STATE     READ WRITE CKSUM
    mfspool                                       ONLINE       0     0     0
      mirror-0                                    ONLINE       0     0     0
        usb-ST3000VX_000-1ES166_000000000000-0:0  ONLINE       0     0     0
        usb-ST3000VX_000-1ES166_000000000000-0:1  ONLINE       0     0     0

errors: No known data errors

  pool: mfspool-ext
 state: ONLINE
status: Some supported features are not enabled on the pool. The pool can
    still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
    the pool may no longer be accessible by software that does not support
    the features. See zpool-features(5) for details.
  scan: none requested
config:

    NAME                                        STATE     READ WRITE CKSUM
    mfspool-ext                                 ONLINE       0     0     0
      usb-ST3000VX_000-1ES166_000000000000-0:2  ONLINE       0     0     0

errors: No known data errors

```

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
