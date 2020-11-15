---
title: zookeeper配置详解
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-15 12:51:40
password:
summary: zookeeper 配置详解
tags:
- zookeeper
- 分布式
categories:
- zookeeper
---

# 基本配置

zookeeper基本配置包括三个`clientPort`，`daraDir`，`tickTime`

1. `clientPort`:
* 表示当前服务器的对外服务端口，客户端可以通过该端口和zookeeper服务器创建连接，一般设置为`2181`;
* 每台服务器都可以配置任意可用端口，同时，集群中的所有服务器不需要保持clientPort端口一致;
* 该参数没有默认值，必须配置，不支持系统属性配置方式{`启动时通过命令行添加`};
* zoo.cfg配置文件一般设定了默认值`2181`，不用修改.

2. `dataDir`:
* 该参数没有默认值，必须配置，不支持系统属性配置方式{`启动时通过命令行添加`};
* 该参数用于配置zookeeper服务器存储快照文件的目录;如果没有配置参数`dataLogDir`，事务日志也会存储在同一个目录中.

3. `tickTime`:
* 该参数有默认值:3000， 单位是毫秒， 可选配置，不支持系统属性配置方式{`启动时通过命令行添加`};
* 该参数时zookeeper的最小时间单元长度，很多运行时的时间间隔都是使用`tickTime`的倍数来表示的;例如，回话最小超时时间默认是`2*tickTime`.


## 高级参数配置

1. `dataLogDir`:
* 该参数有默认值，默认值和`dataDir`目录一致，可选配置，不支持系统属性配置方式{`启动时通过命令行添加`};
* zookeeper在返回客户端的请求前，必须将`事务的日志写入磁盘`，保证数据的一致性，因此对磁盘的写性能要求非常高，因此最好和快照目录分开，且配置成一个单独的磁盘或者挂载点，可以显著的提高zookeeper的性能.

2. `initLimit`:
* 该参数有默认值`10`，表示10倍的`tickTime`，必须是正整数，不支持系统属性配置方式;
* 该参数用于`leader服务器等待follower服务器启动，并完成数据同步的时间`; 例如当一个新的机器加入集群，首先需要与leader建立连接并同步leader服务器的数据，从而确定对外提供服务的起始状态;
* 如果集群数据量规模增大，会出现无法在较短时间内完成同步的问题，这样就必须适当的增大该参数的值.

3. `syncLimit`:
* 该参数有默认值`5`，表示5倍的`tickTime`，必须是正整数，不支持系统属性配置方式;
* 该参数表示Leader服务器和Follower服务器之间`心跳检测的最大延时`，在集群运行过程中，Leader服务器会与所有的Follower进行心跳检测来确定服务器是否存活，如果超过了syncLimit时间都没有收到Follower的心跳包，就会认为该服务器已经脱离同步;
* 通常情况不需要修改该值，如果部署的环境网络环境质量较低{*网络延时较大，丢包严重*}，可以适当增大参数值

4. `snapCount`:
* 该参数有默认值`100000`，可选配置，**仅支持系统属性配置方式**`zookeeper.snapCount`;
* 表示`相邻两次数据快照之间事务的次数`，即事务的次数达到了`snapCount`就会进行`一次数据快照`;

5. `preAllocSize`:
* 该参数有默认值`65536`，单位KB，即`64MB`，可选配置，**仅支持系统属性配置方式**`zookeeper.preAllocSize`;
* 该参数用于配置zookeeper`事务日志文件预分配`的磁盘空间大小;
* 如果`snapCount`参数修改，那么`preAllocSize`参数也应该随之做出变更;例如，snapCount设置为500，同时预估每次事务的数据量大小为至多1KB，那么参数preAllocSize设置为500就足够了.

6. `minSessionTimeout`和`maxSeesionTimeout`
* 这两个参数有默认值，分别是参数 tickTime值的2倍和20倍，即默认的会话超时时间在`2* tickTime~20* tickTime`范图内，单位毫秒，可以不配置，不支持系统属性方式配置。
* 这两个参数用于`服务端对客户端会话的超时时间`进行限制，如果客户端设置的超时时间不在该范围内，那么会被服务端强制设置为最大或最小超时时间。

7. `maxClientCnxns`:
* 该参数有默认值:`60`，可选配置，不支持系统属性方式配置。
* 从Socket层面`限制单个客户端与单台服务器之间的并发连接数`，即以IP地址粒度来进行连接数的限制。如果将该参数设置为0，则表示对连接数不作任何限制。
* 需要注意该连接数限制选项的使用范围，其仅仅是对单台客户端机器与单台ZooKeeper服务器之间的连接数限制，并不能控制所有客户端的连接数总和。
* 在3.40版本以前该参数的默认值都是10，从3.4.0版本开始变成了60，因此运维人员尤其需要注意这个变化，以防Zoo Keeper版本变化带来服务端连接数限制变化的隐患.

8. `jute.maxbuffer`:
* 该参数有默认值:1048575，单位是字节，可以不配置，**仅支持系统属性方式配置**：`jute.maxbuffer`;
* 该参数用于配置单个数据节点\[ZNode\]上可以存储的最大数据量大小。通常情况下，运维人员不需要改动该参数，同时考虑到Zookeeper上不适宜存储太多的数据，往往还需要将该参数设置的更小;
* 需要注意的是，`在变更该参数的时候，需要在ZooKeeper集群的所有机器以及所有的客户端上均设置才能生效`。

9. `clientPortAddress`:
* 该参数没有默认值：可以不配置，不支持系统属性方式配置；
* 针对那些多网卡的机器，该参数允许为每个IP地址指定不同的监听端口.

10. `server.id=host:port:port`
* 该参数没有默认值，在单机模式下可以不配置，不支持系统属性方式配置。
* 该参数用于配置组成ZooKeeper集群的机器列表，其中id即为Server ID，与每台服务器myid文件中的数字相对应。同时，在该数中，会配置两个端口：第一个端口用于指定 Follower服务器与Leader进行运行时通信和数据同步时所使用的端口，第二个端口则专门用于进行 Leader选举过程中的投票通信。
* 在ZooKeeper服务器启动的时候，其会根据myid文件中配置的Server ID来确定自己是哪台服务器，并使用对应配置的端口来进行启动。如果在实际使用过程中，需要在同一台服务器上部暑多个∠o0 Keeper实例来构成伪集群的话，那么这些端口都需要配置成不同，例如:
``` bash
server.1=192.168.0.1:2777:3777
server.2=192.168.0.1:2888:3888
server.3=192.168.0.1:2999:3999
```

11. `autopurge.snapRetainCount`
* 该参数有默认值:`3`，可以不配置，不支持系统属性方式配置。
* 从3.4.0版本开始，Zookeeper提供了对历史事务日志和快照数据自动清理的支持。参数`autopurge.snapRetainCount`用于配置ZooKeeper在自动清理的时候需要保留的快照数据文件数量和对应`autopurge.snapRetainCount`的事务日志文件。需要注意的是，并不是磁盘上的所有事务日志和快照数据文件都可以被清理掉————那样的话将无法恢复数据。因此`autopurge.snapRetainCount`参数的最小值是3，如果配置的`autopurge.snapRetainCount`值比3小的话，那么会被自动调整到3，即至少需要保留3个快照数据文件和对应的事务日志文件。

12. `autopurge.purgeInterval`
* 该参数有默认值:0，单位是小时，可以不配置，不支持系统属性方式配置。
* 参数`autopurge.purgeInterval`和参数`autopurge.snapRetainCount`配套使用，用于配置ZooKeeper进行`历史文件自动清理的频率`。如果配置该值为0或负数，那么就表明不需要开启定时清理功能。 ZooKeeper默认不开启这顼功能。

13. `fsync.warningthresholdms`
* 该参数有默认值:`1000`，单位是毫秒，可以不配置，**仅支持系统属性方式配置**：`fsync.warningthresholdms`
* `fsync.warningthresholdms`用于配置ZooKeeper进行`事务日志fync操作时消耗时间的报警阈值`。一旦进行一个fync操作消耗的时间大于参数`fsync.warningthresholdms`指定的值，那么就在日志中打印出报警日志。

14. `forceSync`
* 该参数有默认值：`yes`，可以不配置，**仅支持系统属性方式配置**：`zookeeper.forceSync`
* 该参数用于配置ZooKeeper服务器是否在事务提交的时候，`将日志写入操作强制刷入磁盘`\[即调用java.nio.channels. FileChannel.force接口\]，默认情况下是“yes”，即每次事务日志写入操作都会实时刷入磁盘。如果将其设置为“no”，则能一定程度的提高 ZooKeeper的写性能，但同时也会存在类似于机器断电这样的安全风险。

15. `globalOutstandingLimit`
* 该参数有默认值:`1000`，可以不配置，**仅支持系统属性方式配置**：`zookeeper.globalOutstandingLimit`
* 参数`globalOutstandingLimit`用于配置ZooKeeper服务器`最大请求堆积数量`。在Zookeeper服务器运行的过程中，客户端会源源不断的将请求发送到服务端，为了防止服务端資源\[包括CPU、参数内存和网络等\]耗尽，服务端必须限制同时处理的请求数，即最大请求堆积数量

16. `leaderServes`
* 该参数有默认值：yes，可以不配置，可选配置项为“yes”和“no"，**仅支持系统属性方式配置**：`zookeeper.leaderServes`
* 该参数用于配置 Leader服务器是否能够接受客户端的连接，即是否允许 Leader向客户端提供服务，默认情况下， Leader服务器能够接受并处理客户端的所有读写请求。在Zookeeper的架构设计中，Leader服务器主要用来进行对事务更新请求的协调以及集群本身的运行时协调，因此，可以设置让 Leader服务器不接受客户端的连接，以使其专注于进行分布式协调。

17. `SkipAcl`
* 该参数有默认值：no，可以不配置，可选配置项为“yes”和“no”，**仅支持系统属性方式配置**：`zookeeper.skipACL
* 该参数用于配置 ZooKeeper服务器是否跳过ACL权限检查，默认情况下是“no”，即会对每一个客户端请求进行权限检查。如果将其设置为yes”，则能一定程度的提高ZooKeeper的读写性能，但同时也将向所有客户端开放ZooKeeper的数据，包括那些之前设置过ACL权限的数据节点，也将不再接受权限控制。

18. `cnxTimeout`
* 该参数有默认值:5000，单位是毫秒，可以不配置，**仅支持系统属性方式配置**：`zookeeper.cnxTimeout`
* 该参数用于配置在 Leader选举过程中，各服务器之间进行TCP连接创建的超时时间。

19. 