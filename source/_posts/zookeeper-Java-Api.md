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

## 创建会话

### 创建基本的会话

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

### 创建复用的会话连接

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

## 创建节点

### 构造方法

``` java
// 同步
String create(final String path, byte data[], List<ACL> acl, CreateMode createMode)
// 异步
void create(final String path, byte data[], List<ACL> acl, CreateMode createMode, StringCallback cb, Object ctx)
```

* `path` 创建的数据节点的节点路径，例如，/zk-book/fool
* `data[]` 字节数组，是节点创建后的初始内容
* `acl` 节点的ACL策略
  * `Ids.OPEN_ACL_UNSAFE`：完全开放
  * `Ids.CREATOR_ALL_ACL`：创建该znode的连接拥有所有权限
  * `Ids.READ_ACL_UNSAFE`：所有的客户端都可读
* `createMode` 节点类型
  * `CreateMode.EPHEMERAL`: 临时
  * `CreateMode.EPHEMERAL_SEQUENTIAL`: 临时顺序
  * `CreateMode.PERSISTENT`: 持久
  * `CreateMode.PERSISTENT_SEQUENTIAL`: 持久顺序
* `cb` 异步回调函数
* `ctx` 传递一个对象，通常是一个上下文（Context）信息

### 使用同步方法创建一个节点

``` java
package com.hulin.zk;

import org.apache.zookeeper.*;

import java.util.concurrent.CountDownLatch;

public class CreateSyncNode implements Watcher {

    private static CountDownLatch downLatch = new CountDownLatch(1);

    public static void main(String[] args) throws Exception {
        ZooKeeper zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new CreateSyncNode());
        downLatch.await();
        String path1 = zooKeeper.create("/zk-test-ephemeral",
                "123".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL);
        System.out.println("Success created znode" + path1);

        String path2 = zooKeeper.create("/zk-test-ephemeral-",
                "456".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL_SEQUENTIAL);
        System.out.println("Success created znode" + path2);
    }

    public void process(WatchedEvent watchedEvent) {
        if (Event.KeeperState.SyncConnected == watchedEvent.getState()) {
            downLatch.countDown();
        }
    }
}
```

输出

``` java
Success created znode/zk-test-ephemeral
Success created znode/zk-test-ephemeral-0000000006
```

### 使用异步创建节点

``` java
package com.hulin.zk;

import org.apache.zookeeper.*;

import java.util.concurrent.CountDownLatch;

public class CreateAsyncNode implements Watcher {
    private static CountDownLatch count = new CountDownLatch(1);

    public static void main(String[] args) throws Exception {
        ZooKeeper zooKeeper = new ZooKeeper("10.28.200.233:2181",
                5000,
                new CreateAsyncNode());
        count.await();

        zooKeeper.create("/zk-test-ephemeral-",
                "123".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL,
                new MyStringCallback(), "I am context");

        zooKeeper.create("/zk-test-ephemeral-",
                "456".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL,
                new MyStringCallback(), "I am context");

        zooKeeper.create("/zk-test-ephemeral-",
                "789".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL_SEQUENTIAL,
                new MyStringCallback(), "I am Context");
        Thread.sleep(Integer.MAX_VALUE);
    }

    public void process(WatchedEvent watchedEvent) {
        if (Event.KeeperState.SyncConnected == watchedEvent.getState()) {
            count.countDown();
        }
    }
}


import org.apache.zookeeper.AsyncCallback;

public class MyStringCallback implements AsyncCallback.StringCallback {
    public void processResult(int rc, String path, Object ctx, String name) {
        System.out.println("Create path result: [" + rc + ", " + path + ", "
                + ctx + "，real path name:" +name);
    }
```

返回结果

``` java
Create path result: [0, /zk-test-ephemeral-, I am context，real path name:/zk-test-ephemeral-
Create path result: [-110, /zk-test-ephemeral-, I am contex，treal path name:null
Create path result: [0, /zk-test-ephemeral-, I am Context，real path name:/zk-test-ephemeral-0000000009
```

### 异步回调参数详解

``` java
void processResult(int rc, String path, Object ctx, String name);
```

* `rc` 服务端响应码: ResultCode
  * `0`：接口调用成功
  * `-4`：客户端和服务端连接已断开
  * `-110`：指定节点已存在
  * `-112`：会话已过期
* `path` 节点路径
* `ctx` 接口调用时传入的ctx值
* `name` 实际的在服务端创建的节点名

## 读取数据

### 获取子节点

#### getChildren构造方法

``` java
```

#### 同步方法

``` java
package com.hulin.zk;

import org.apache.zookeeper.*;

import java.util.concurrent.CountDownLatch;

public class MyGetChildren implements Watcher {
    private static final CountDownLatch count = new CountDownLatch(1);
    private static ZooKeeper zooKeeper;

    public static void main(String[] args) throws Exception {
        // 使用集群模式，且默认的根目录是/hulin
        zooKeeper = new ZooKeeper("10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/hulin",
                5000,
                new MyGetChildren());
        count.await();

        zooKeeper.create("/mynode/node1",
                "123".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL);

        zooKeeper.getChildren("/mynode", true);

        zooKeeper.create("/mynode/node2",
                "123".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.EPHEMERAL);
        Thread.sleep(Integer.MAX_VALUE);
    }

    public void process(WatchedEvent event) {
        System.out.println("-------");
        if (Event.KeeperState.SyncConnected == event.getState()) {
            if (Event.EventType.None == event.getType() && null == event.getPath()) {
                count.countDown();
            } else if (Event.EventType.NodeChildrenChanged == event.getType()) {
                try {
                    System.out.println("ReGet Child:" + zooKeeper.getChildren(event.getPath(),true));
                } catch (KeeperException e) {
                    e.printStackTrace();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

输出

``` java
-------
-------
ReGet Child:[node2, node1]
```

可以看到第一次创建的节点的时候，没有订阅子节点变化的通知，所以没有打印子节点变化的信息
