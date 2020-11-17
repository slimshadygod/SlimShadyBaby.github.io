---
title: win10安装anaconda环境
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-14 20:00:22
password:
summary: win10 anaconda环境安装
tags:
- win10
- anaconda
categories:
- python
---

## 下载Anaconda

1. 下载地址: <https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/>, 选择windows64位安装包,如下图所示
    ![安装包](install.png)

2. 安装到非C盘的路径,比较占用空间

3. 添加系统环境变量

    ``` shell
    E:\anaconda3
    E:\anaconda3\Scripts
    ```

## 查看和更新包

1. 进入`E:\anaconda3\Scripts`目录,里面包含了pip.exe程序,在该目录执行cmd,然后`pip list`可以看到anaconda安装的所有包

2. 更换pip源

    ``` bash
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --set show_channel_urls yes
    ```

3. 临时更换源

    ``` bash
    pip install ssl -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
    ```

## 遇到的问题

1. anaconda 的pip 和系统的python3 的pip冲突

    ``` bash
    1.添加环境变量
    E:\anaconda3
    E:\anaconda3\Scripts
    E:\anaconda3\Library\bin

    2.修改Script下的pip.exe为pip3.8.exe

    3.执行pip3.8 list即可查看对应的安装包
    ```

2. 更换pip镜像

    默认的pip镜像下载太慢,修改为其他源

    ``` bash
    1. 修改为清华源
    通过everthing找到pip.ini文件,修改为如下值:
    [global]
    index-url = https://pypi.tuna.tsinghua.edu.cn/simple

    2. 更换后还是报错https无法访问
    打开网址：https://slproweb.com/products/Win32OpenSSL.html
    找到Win64 OpenSSL v1.1.1h Ligh,只有3M左右,下载安装即可
    ```

3. 其他问题

    ``` shell
    1.升级setuptools
    E:\anaconda3>python -m pip install --upgrade setuptools -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
    Looking in indexes: http://pypi.douban.com/simple

    2.升级pip
    python -m pip install --upgrade pip

    3.以user mode 升级
    pip install --user --upgrade pillow -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
    ```
