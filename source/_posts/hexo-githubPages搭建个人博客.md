---
title: Hexo+githubPages搭建个人博客
top: false
cover: true # v1.0.2版本新增，表示该文章是否需要加入到首页轮播封面中
toc: true
mathjax: true
date: 2020-08-11 13:33:24
password:
summary: hexo + github pages 搭建个人博客简略教程
tags:
- blog
- hexo
- github
categories:
- blogs
---

## 目录结构

- 安装node.js
- 安装git
- 注册github账号
- 安装hexo
- 写文章、发布文章
- 绑定域名

## 安装git

1. 生成新的`SSH Key`

> ssh-keygen -t rsa -b 4096 -C "您的Github邮箱地址"

## 绑定域名

可以选择自己喜欢的服务商，以下已阿里云为例，首先下载一个`阿里云`APP，在`域名注册`页面输入自己想要的域名，如我的`slimshadylin`，然后选择一个自己喜欢的后缀，不同的后缀有不同的价格，后续根据提示操作即可

然后找到自己购买的域名页面
![域名管理](domain.jpg)

按照如下进行编辑,github的ip地址，自己ping以下xxx.github.io即可
![域名解析](domain1.jpg)
![域名解析](domain2.jpg)

最后修改githubPages设置

- 进入`github pages`对应的仓库首界面
- 点击`Settings`，进入这个仓库的设置界面
- 找到`GitHub Pages`配置栏，勾选`Enforce HTTPS`,如果不可勾选，可能是阿里云的域名认证没有通过，或者是域名刚买，等两个小时就可以操作了

最后就可以通过自己的域名，愉快的访问自己的博客啦
