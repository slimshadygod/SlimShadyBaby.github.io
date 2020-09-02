---
title: Dockerfile基本操作
top: false
cover: false
toc: true
mathjax: true
date: 2020-09-02 22:47:38
password:
summary: Dockerfile 基本操作
tags:
- docker
categories:
- docker
---

## 常用命令

1. FROM imaage_name:tag
    定义了使用哪个基础镜像启动构建流程

2. MAINTAINER user_name
    声明镜像的创建者

3. ENV key value
    设置环境变量(可以写多条)

4. RUN command
    Dockerfile的核心部分(可以写多条)

5. ADD source_dir/file dest_dir/file
    将宿主机文件复制到容器内，如果是一个压缩文件，将会在复制后自动解压

6. COPY source_dir/file dest_dir/file
    和add相似，但是如果有压缩文件并不能解压

7. WORKDIR path_dir
    设置工作目录
