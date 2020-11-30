---
title: github克隆太慢问题
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-30 21:06:27
password:
summary:
tags:
- github
categories:
- github
---

## 解决github克隆太慢的问题

### 思路

git clone特别慢是因为github.global.ssl.fastly.net域名被限制了。
只要找到这个域名对应的ip地址，然后在hosts文件中加上ip–>域名的映射，刷新DNS缓存便可。

### 执行步骤

1. 打开网址<https://github.com.ipaddress.com/>，分别搜索

    ``` bash
    github.global.ssl.fastly.net
    github.com
    ```

2. 得到ip

    ![github.com](domain.png)

3. 写入hosts

    ![hosts](hosts.png)

    ``` bash
    199.232.69.194  github.global-ssl.fastly.net
    140.82.114.4  github.com
    ```

4. 刷新缓存
    打开cmd，执行如下命令

    ``` cmd
    ipconfig /flushdns
    ```
