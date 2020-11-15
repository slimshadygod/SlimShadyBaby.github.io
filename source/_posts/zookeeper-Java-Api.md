---
title: zookeeper-Java-Api
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-15 18:12:32
password:
summary: zookeeper java-API
tags:
- zookeeper
- 分布式
- java
categories:
- zookeeper
---

# 创建会话

## 创建基本的会话

``` java
package com.hulin.zk;

import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.ZooKeeper;

import java.io.IOException;
import java.util.concurrent.CountDownLatch;

public class MyCreateSession implements Watcher {
    private static CountDownLatch countDownLatch = new CountDownLatch(1);

    public static void main(String[] args) throws Exception {
        ZooKeeper zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new MyCreateSession());
        System.out.println(zooKeeper.getState());
        try {
            countDownLatch.await();
        } catch (InterruptedException e) {
            System.out.println("Zookeeper Session established");
        }
    }

    public void process(WatchedEvent watchedEvent) {
        System.out.println("received watched event:" + watchedEvent);
        if (Event.KeeperState.SyncConnected == watchedEvent.getState()){
            countDownLatch.countDown();
        }
    }
}

```

输出
``` java
CONNECTING
received watched event:WatchedEvent state:SyncConnected type:None path:null
```

## 创建复用的会话连接

``` java
package com.hulin.zk;

import org.apache.zookeeper.WatchedEvent;
import org.apache.zookeeper.Watcher;
import org.apache.zookeeper.ZooKeeper;

import java.util.concurrent.CountDownLatch;

public class MyCreateUsageSession implements Watcher {

    private static CountDownLatch countDownLatch = new CountDownLatch(1);

    public static void main(String[] args) throws Exception {
        ZooKeeper zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new MyCreateUsageSession());
        countDownLatch.await();
        long sessionID = zooKeeper.getSessionId();
        byte[] password = zooKeeper.getSessionPasswd();

        //Use illegal SessionId and SessionPassWd
        zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new MyCreateSession(),
                1L,
                "test".getBytes());

        //Use correct SessionId and SessionPassWd
        zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new MyCreateSession(),
                sessionID,
                password);
        Thread.sleep(Integer.MAX_VALUE);
    }

    public void process(WatchedEvent watchedEvent) {
        System.out.println("received watched event:" + watchedEvent);
        if (Event.KeeperState.SyncConnected == watchedEvent.getState()) {
            countDownLatch.countDown();
        }
    }
}
```

输出
``` java
received watched event:WatchedEvent state:SyncConnected type:None path:null
received watched event:WatchedEvent state:Disconnected type:None path:null
received watched event:WatchedEvent state:SyncConnected type:None path:null
```