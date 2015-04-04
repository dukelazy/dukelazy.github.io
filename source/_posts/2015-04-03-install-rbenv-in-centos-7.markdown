---
layout: post
title: "install rbenv in CentOS 7"
date: 2015-04-03 08:43:08 +0800
updated: 2015-04-04 15:43:08 +0800
comments: true
tags: [rbenv,ruby,linux,centos]
categories: 
---

*First Header*  | *Second Header*
------------- | ------------:
Content Cell  | 1
Content Cell  | 50

*Year* | *Temperature (low)* | *Temperature (high)*
-|:-:|:-:
1900 | -10 | 25
1910 | -15 | 30
1920 | -10 | 32

``` bash
cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
source ~/.bash_profile 
```

### Install Ruby 1.9.3 ###
Next install Ruby 1.9.3 and you'll be all set.
```
rbenv install 1.9.3-p551
rbenv local 1.9.3-p551
rbenv rehash 
```

- ftp://ftp.ruby-lang.org/pub/ruby/
- http://cache.ruby-lang.org/pub/ruby/1.9/
- https://ruby-china.org/wiki/rbenv-guide
- http://octopress.org/docs/setup/rbenv/

