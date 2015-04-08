---
layout: post
title: "Multipart/form-data POST Format"
updated: 20154-04-04 20:50:47 +0800
categories: "webapi"
tags: ["http","https","webpai","form-data"]
---

之前用 POST 方式调用很多API都没有问题，这次调作用总是失败，才发现原来这个 WebAPi 需要用 `Multipart/form-data` 格式调用才可以，记录下用普通POST和 `Multipart/form-data` 格式调用的抓包数据。更新现在已经可以支持普通的POST了。

<!--more-->

一、General
--------------------

```
POST /oauth/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: api.s.gitcafe.org
Content-Length: 234
Expect: 100-continue
grant_type=password&username=raven@gitcafe.com&password=[passwordValue]&client_id=0aeef87b0f41e96adde4f323012997961faec5f06210865f7a73c638e542f2bd&client_secret=276ab98bbe499b5c827523b6ad1b095290dbd7e86b98211f84d7d951505389ef&scope=read write

```

二、Multipart/form-data
-------------------------------

```
POST /oauth/token HTTP/1.1
Content-Type: multipart/form-data; boundary="b0e86de3-4c49-42ad-b971-034f42b6634a"
Host: api.s.gitcafe.org
Content-Length: 1021
Expect: 100-continue
Connection: Keep-Alive

--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="grant_type"

password
--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="username"

raven@gitcafe.com
--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="password"

[passwordValue]
--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="client_id"

0aeef87b0f41e96adde4f323012997961faec5f06210865f7a73c638e542f2bd
--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="client_secret"

276ab98bbe499b5c827523b6ad1b095290dbd7e86b98211f84d7d951505389ef
--b0e86de3-4c49-42ad-b971-034f42b6634a
Content-Type: text/plain; charset=utf-8
Content-Disposition: form-data; name="scope"

read write
--b0e86de3-4c49-42ad-b971-034f42b6634a--
```
