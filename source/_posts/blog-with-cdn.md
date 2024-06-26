---
title: blog-with-cdn
author: LC
toc: true
date: 2024-04-24 21:22:34
img:
top:
cover:
password:
summary: 测试发布在github pages 的博客自定义域名并且在cdn中分发
categories:
tags:
---
## 1. 前置
当前托管在 github 的访问地址是`https://myrepo.github.io` 
Note：
此时自己的仓库名应该和自己的域名相同`myrepo.github.io`; 同时配置 blog 应用中关于github pages 发布中关于url的配置)

然后配置自定义域名是 `blog.mydomain.cn`； 可以在 UI 配置，也可以通过直接在 main 分支source文件夹下创建一个位于顶级的文件`CNAME`,其中只存在这个自定义域名，然后发布一次。
此时需要在自己的域名管理添加 DNS 的 CNAME 解析记录，对应以上；即： `blog.mydomain.cn  CNAME  myrepo.github.io`

## 2. cdn 加速
测试使用的cdn产品是 `cdn.cdn5.com`, `sudun.com`

加速域名：km.mydomain.cn
源站：blog.mydomain.cn  OR  myrepo.github.io
cdn域名别名：xxxxxxx

**配置：**
为`加速域名`，在自己的域名管理添加 DNS 的 CNAME 解析记录，对应为：`km.mydomain.cn CNAME cdn域名别名`

**流量：**
加速域名 --> DNS --> cdn域名别名 --> 源站（github.io|blog.cn ）

其中，回源 是指 cdn 向源站请求的过程。回源`host` 这个参数需要源站能够匹配。所以在回源设置中需要保证`host`的值正确;
此例是将值自定义为源站的 servername，`blog.mydomain.cn | myrepo.github.io`

**其它：**
默认使用加密传输，配置了证书、开启了重定向跳转https

**结果：**
国内访问 km.mydomain.cn 可以快速访问，不因为墙的原因而慢了


## 续：试用 cloudflare

之前的免费cdn只有一个月的试用时长，过期之后尝试使用 cloudflare 代理。 \
当前基本情况：
- github pages：发布在 myrepo.github.io, 但是同时使用 GitHub 的自定义域名功能，并强制 https。
- cloudflare： 免费套餐服务，代理一个 blog 域名例如 km.mydomain.cn，它是 CNAME 类型，是 myrepo.github.io 的别名。
- DNS： 使用 cloudflare 管理域名，除去上述需要代理，其他地址仅 DNS 解析即可。

按照常理，经上述配置之后已经可以正常访问加速后的博客（cloudflare ssl 安全建议选择 完全 级别）

## 再续： cloudflare pages

因为 cloudflare 的 cdn 加速没玩明白，所以使用其 pages 功能。流程简明易操作：对接 git 仓库并，选择仓库与分支，部署，设置自定义域名，CNAME 解析即可。 \
访问快于 github pages，此段内容用于检测其是否能够自动更新。