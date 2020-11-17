---
title: zookeeper节点操作
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-15 17:35:21
password:
summary: zookeeper 节点操作
tags:
- zookeeper
- 分布式
categories:
- zookeeper
---

## 创建

使用`create`命令可以创建Zookeeper节点，用法如下：
> create [-s] [-e] path data acl

* -s 表示顺序节点
* -e 表示临时节点
* path表示节点的路径
* data表示节点的数据
* acl表示节点的 ACL 策略。
* 默认创建的是持久节点

``` bash
// 进入zk客户端工具
[bbd@shucang03 bin]$ sh zkCli.sh
// 查看create帮助信息
[zk: localhost:2181(CONNECTED) 1] create
create [-s] [-e] [-c] [-t ttl] path [data] [acl]
// 创建一个节点
[zk: localhost:2181(CONNECTED) 2] create /zk-book 123
Created /zk-book
```

## 读取

### ls

1. 使用ls命令，可以查看指定路径下的节点

    ``` bash
    [zk: localhost:2181(CONNECTED) 3] ls
    ls [-s] [-w] [-R] path
    [zk: localhost:2181(CONNECTED) 4] ls /
    [zk-book, zookeeper]
    ```

    可以看到，zookeeper服务启动会在根目录下，默认创建一个叫做`zookeeper`的保留节点

### get

1. `get`，获取指定节点的数据内容和属性
    >get [-s] [-w] path

    ``` bash
    [zk: localhost:2181(CONNECTED) 5] get /zk-book
    123
    [zk: localhost:2181(CONNECTED) 7] get
    get [-s] [-w] path
    [zk: localhost:2181(CONNECTED) 8] get -s -w /zk-book
    123
    cZxid = 0x200000002 // 创建节点的事务ID
    ctime = Sun Nov 15 17:42:43 CST 2020
    mZxid = 0x200000002 // 最后一次更新节点的事务ID
    mtime = Sun Nov 15 17:42:43 CST 2020
    pZxid = 0x200000002
    cversion = 0
    dataVersion = 0
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 3
    numChildren = 0
    ```

    可以看到保留节点是没有内容的
    ![节点操作](get.png)

## 更新

1. `set`,更新节点数据

    ``` bash
    [zk: localhost:2181(CONNECTED) 10] set /zk-book 2333
    WATCHER::
    WatchedEvent state:SyncConnected type:NodeDataChanged path:/zk-book
    [zk: localhost:2181(CONNECTED) 11] get -s -w /zk-book
    2333
    cZxid = 0x200000002
    ctime = Sun Nov 15 17:42:43 CST 2020
    mZxid = 0x200000003 // 更新节点后，事务ID+1
    mtime = Sun Nov 15 17:54:35 CST 2020
    pZxid = 0x200000002
    cversion = 0
    dataVersion = 1
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 4
    numChildren = 0
    [zk: localhost:2181(CONNECTED) 12]
    ```

* 可以看到，更新节点后`mZxid`+1，`dataVersion`+1

## 删除

1. `delete`，删除节点信息

    ``` bash
    [zk: localhost:2181(CONNECTED) 13] delete /zk-book
    WATCHER::
    WatchedEvent state:SyncConnected type:NodeDeleted path:/zk-book
    [zk: localhost:2181(CONNECTED) 14] ls /
    [zookeeper]
    [zk: localhost:2181(CONNECTED) 15]
    ```
