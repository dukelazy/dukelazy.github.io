---
layout: post
title: "install Octopress in CentOS 7"
date: 2015-04-03 08:42:43 +0800
comments: true
categories: 
---
```
git clone https://github.com/imathis/octopress.git octopress

cd octopress
rbenv local 1.9.3-p551
rbenv rehash 
ruby --version

gem install bundler
rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
可以编辑Gemfile，使用淘宝的源
bundle install -V 或者 bundle install -V

[dukelazy@localhost octopress]$ rake install
## Copying classic theme into ./source and ./sass
mkdir -p source
cp -r .themes/classic/source/. source
mkdir -p sass
cp -r .themes/classic/sass/. sass
mkdir -p source/_posts
mkdir -p public
```

- error
```
[dukelazy@localhost octopress]$ gem install bundler
ERROR:  Loading command: install (LoadError)
    cannot load such file -- zlib
ERROR:  While executing gem ... (NameError)
    uninitialized constant Gem::Commands::InstallCommand
```
```
sudo yum install zlib-devel
rbenv uninstall 1.9.3-p551
rbenv install 1.9.3-p551
rbenv rehash
```
- error
```
[dukelazy@localhost octopress]$ bundle install
Could not load OpenSSL.
You must recompile Ruby with OpenSSL support or change the sources in your
Gemfile from 'https' to 'http'. Instructions for compiling with OpenSSL using
RVM are available at rvm.io/packages/openssl.

sudo yum install openssl-devel
rbenv install 1.9.3-p551
rbenv rehash
```
