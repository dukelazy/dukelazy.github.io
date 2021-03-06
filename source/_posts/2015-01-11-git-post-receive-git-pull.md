---
Language: zh_CN
Author: Shepherd Lazy
Create_by: Shepherd Lazy <shepherd.lazy@Gmail.com>
Date_Time: 2015-1-11 3:13:14 
layout: post
title: "[转]解决在git的post-receive钩子不能执行git pull"
Subtitle: 
categories: "git"
tags:  ["git","hook"]
Description:  
---

有一个需求是本地git在push到远程 git repo 之后，在远程服务器上自动在/dir/foo下执行 git pull 的操作。想来是一个很简单的需求，不就是在远程的 foo.git 仓库中的 hook 里加一个 post-receive 的钩子，然后在钩子里加入一个 git pull 的操作。但是实际操作的时候发现有问题的，因为这样忽略了一个小细节的问题。

<!--more-->

操作之前，头脑里想的代码如下：

``` bash
#!/bin/sh
cd /var/git/web3/etc/puppet
/usr/bin/git pull
```

用这个代码在 git push ssh://git@ownlinux.org:/opt/foo.git 之后，发现远程服务器上的 /dir/foo 目录下并没有成功 pull 到最新的数据，并且终端上也有报错（remote: fatal: Not a git repository: ‘.’）。 后面发现 git 的钩子在运行的时候会调用 GIT_DIR 这个环境变量，而不是PWD 这个。所以在 git pull 的时候提示 Not a git repository: ‘.’ ，其中 “.” 正是 GIT_DIR 这个环境变量的值。

钩子的代码改成下面的之后，运行正常了：

``` sh
#!/bin/sh
unset $(git rev-parse --local-env-vars)
cd /var/git/web3/etc/puppet
/usr/bin/git pull
```

*参考资料*
------------------------------
- *[OwnLinux.org](http://www.ownlinux.org). [记 git 的 post-receive 钩子不能正常 git pull 的问题](http://www.ownlinux.org/2013/11/22/git-post-receive-git-pull.html). 2013-11-22 [205-01-11]*
