---
title: kafka安装
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-17 21:58:09
password:
summary: kafka 安装教程
tags:
- kafka
categories:
- kafka
---

## 下载

官网下载地址：<https://www.apache.org/dyn/closer.cgi?path=/kafka/2.6.0/kafka_2.13-2.6.0.tgz>

### 安装

1. 下载

    ``` linux
    [bbd@shucang01 hulin]$ wget https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/2.6.0/kafka_2.13-2.6.0.tgz
    [bbd@shucang01 hulin]$ ls
    kafka_2.13-2.6.0.tgz
    ```

2. 解压

    ``` linux
    [bbd@shucang01 hulin]$ tar -xzvf kafka_2.13-2.6.0.tgz
    ...
    ...
    kafka_2.13-2.6.0/libs/rocksdbjni-5.18.4.jar
    kafka_2.13-2.6.0/libs/kafka-streams-scala_2.13-2.6.0.jar
    kafka_2.13-2.6.0/libs/kafka-streams-test-utils-2.6.0.jar
    kafka_2.13-2.6.0/libs/kafka-streams-examples-2.6.0.jar
    ```

3. 添加环境变量：`vi /etc/profile`

    ``` linux
    export KAFKA_HOME=/home/bbd/hulin/kafka_2.13-2.6.0
    export ZK_HOME=/home/bbd/hulin/apache-zookeeper-3.6.2-bin
    export PATH=$PATH:$KAFKA_HOME/bin:$ZK_HOME
    ```

4. 修改配置文件 `$KAFKA_HOME/conf/server.properties`

    ``` linux
    # 集群中每个brokerid需要不同
    broker.id=0
    # broker对外提供服务的地址
    listeners=PLAINTEXT://10.28.200.233:9092
    # 存放消息日志文件的地址
    log.dirs=/data1/hulin/kafka-logs
    # zk地址
    zookeeper.connect=10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/kafka
    ```

5. 修改其他机器

    ``` linux
    scp -P 51668 -r kafka_2.13-2.6.0 bbd@10.28.200.234:`pwd`
    # 集群中每个brokerid需要不同
    broker.id=2
    # broker对外提供服务的地址
    listeners=PLAINTEXT://10.28.200.234:9092
    # 存放消息日志文件的地址
    log.dirs=/data1/hulin/kafka-logs
    # zk地址
    zookeeper.connect=10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/kafka

    scp -P 51668 -r kafka_2.13-2.6.0 bbd@10.28.200.235:`pwd`
    # 集群中每个brokerid需要不同
    broker.id=3
    # broker对外提供服务的地址
    listeners=PLAINTEXT://10.28.200.235:9092
    # 存放消息日志文件的地址
    log.dirs=/data1/hulin/kafka-logs
    # zk地址
    zookeeper.connect=10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/kafka
    ```

6. 启动

    ``` linux
    kafka-server-start.sh -daemon /home/bbd/hulin/kafka_2.13-2.6.0/config/server.properties
    或者
    kafka-server-start.sh /home/bbd/hulin/kafka_2.13-2.6.0/config/server.properties &
    ```

7. 查看kafka进程 `jps -l`

    ``` linux
    [bbd@shucang02 config]$ jps -l
    29473 kafka.Kafka

    如果没有出现kafka.Kafka进程，前台启动，然后看日志报错解决即可，有可能是配置文件没有写对，或者是文件的权限不足
    ```

8. 注意启动的时候保持`logs.dir`文件夹是空的，不然可能无法启动

## 生产与消费

1. 创建一个分区为4，副本因子为3的主题

    ``` linux
    [bbd@shucang01 ~]$ kafka-topics.sh --zookeeper 10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/kafka --create --topic topic-demo --replication-factor 3 --partitions 4

    Created topic topic-demo.
    ```

2. 查看分区信息

    ``` linu
    [bbd@shucang01 ~]$ kafka-topics.sh --zookeeper 10.28.200.233:2181,10.28.200.234:2181,10.28.200.235:2181/kafka --describe --topic topic-demo
    Topic: topic-demo   PartitionCount: 4   ReplicationFactor: 3    Configs:
    Topic: topic-demo   Partition: 0    Leader: 0    Replicas: 0,1,2    Isr: 0,1,2
    Topic: topic-demo   Partition: 1    Leader: 1    Replicas: 1,2,0    Isr: 1,2,0
    Topic: topic-demo   Partition: 2    Leader: 2    Replicas: 2,0,1    Isr: 2,0,1
    Topic: topic-demo   Partition: 3    Leader: 0    Replicas: 0,2,1    Isr: 0,2,1
    [bbd@shucang01 ~]$
    ```

3. 生产消息
    新版的kafka使用--bootstrap-server时，端口号使用`9092`，不然无法访问

    ``` linux
    [bbd@shucang01 ~]$ kafka-console-producer.sh --bootstrap-server 10.28.200.233:9092,10.28.200.234:9092,10.28.200.235:9092 --topic topic-demo
    -> Hello,Kafka
    ```

4. 消费消息

    ``` linux
    [bbd@shucang01 ~]$ kafka-console-consumer.sh --bootstrap-server 10.28.200.233:9092,10.28.200.234:9092,10.28.200.235:9092 --topic topic-demo
    Hello,Kafka
    ```
