---
title: windows安装mysql5.7
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-30 21:06:46
password:
summary: windows下安装mysql-5.7
tags:
- mysql
categories:
- mysql
---


## 下载mysql

1. 进入[mysql](https://downloads.mysql.com/archives/community/)官网

    ![下载](mysqldownload.png)

2. 解压缩到D盘

    ![解压缩](unzip.png)

3. 添加系统环境变量

    ``` bat
    D:\mysql-5.7.31\bin;
    ```

## 修改配置

1. 新增`my.ini`文件，写入以下内容：

    ``` ini
    [mysql]

    #设置mysql客户端默认字符集

    default-character-set=utf8

    [mysqld]

    #设置3306端口

    port = 3306

    #设置mysql的安装目录

    basedir=D:\mysql-5.7.31-winx64

    #设置mysql数据库的数据的存放目录

    datadir=D:\mysql-5.7.31-winx64\data

    #允许最大连接数

    max_connections=200

    #服务端使用的字符集默认为8比特编码的latin1字符集

    character-set-server=utf8

    #创建新表时将使用的默认存储引擎

    default-storage-engine=INNODB
    ```

## 启动服务

1. 以管理员的身份进入`cmd`，依次执行如下命令：

    ``` cmd
    mysqld install
    net start mysql
    mysqld --initialize-insecure --user=mysql
    ```

    ``` mysql
    C:\Windows\system32>d:

    D:\>cd mysql-5.7.31-winx64

    D:\mysql-5.7.31-winx64>cd bin

    D:\mysql-5.7.31-winx64\bin>

    D:\mysql-5.7.31-winx64\bin>mysqld install
    Service successfully installed.

    D:\mysql-5.7.31-winx64\bin>net start mysql
    MySQL 服务正在启动 .
    MySQL 服务无法启动。

    服务没有报告任何错误。

    请键入 NET HELPMSG 3534 以获得更多的帮助。

    D:\mysql-5.7.31-winx64\bin>mysqld --initialize-insecure --user=mysql
    mysqld: Can't create directory 'D:\mysql-5.7.31\data\' (Errcode: 2 - No such fil
    e or directory)
    2020-11-30T13:32:03.617118Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is
    deprecated. Please use --explicit_defaults_for_timestamp server option (see doc
    umentation for more details).
    2020-11-30T13:32:03.625119Z 0 [ERROR] Can't find error-message file 'D:\mysql-5.
    7.31\share\errmsg.sys'. Check error-message file location and 'lc-messages-dir'
    configuration directive.
    2020-11-30T13:32:03.629119Z 0 [ERROR] Aborting

    # 如果报以上错误，多半是ini中data目录配置的不对，需检查一下
    D:\mysql-5.7.31-winx64\bin>

    D:\mysql-5.7.31-winx64\bin>mysqld --initialize-insecure --user=mysql

    D:\mysql-5.7.31-winx64\bin>net start mysql
    MySQL 服务正在启动 .
    MySQL 服务已经启动成功。

    # 默认密码为空
    D:\mysql-5.7.31-winx64\bin>mysql -u root -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 4
    Server version: 5.7.31 MySQL Community Server (GPL)

    Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql>
    ```

## 常用命令

1. 安装完成后查看编码:

    ``` mysql
    show variables like 'character_set%'
    ```

    ![查看编码](charset.png)

2. 修改默认密码，默认密码为空:

    ``` mysql
    SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
    ```

    ![修改密码](modify_pd.png)
