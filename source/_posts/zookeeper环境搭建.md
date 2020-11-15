---
title: zookeeper环境搭建
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-15 12:51:28
password:
summary: zookeeper 环境搭建
tags:
- zookeeper
- 分布式
categories:
- zookeeper
---

# 集群模式

## 下载ZooKeeper
1. 官方网站: https://zookeeper.apache.org/releases.html
2. 一定要下载bin.tar.gz文件,另外一个是源码包
``` bash
// 下载安装包
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz
// 解压
tar -xzvf apache-zookeeper-3.6.2-bin.tar.gz
```

## 修改配置文件
1. 修改`"%ZK_HOME%"/conf`下的`zoo.cfg`
``` bash
// 进入conf目录
cd apache-zookeeper-3.6.2-bin/conf
// 修改zoo_sample.cfg文件名
mv zoo_sample.cfg zoo.cfg
// 编辑文件
vim zoo.cfg
// 添加内容
dataDir=/data1/hulin/zookeeper/data
dataLogDir=/data1/hulin/zookeeper/log
server.1=10.28.200.233:2888:3888
server.2=10.28.200.234:2888:3888
server.3=10.28.200.235:2888:3888
#开启四字命令
4lw.commands.whitelist=*
// 在dataDir目录下新建一个myid文件，并写入对应的server.ID的id
touch /data1/hulin/zookeeper/data/myid
echo 1 > /data1/hulin/zookeeper/data/myid
```

![配置信息](zoocfg.png)

* `df -lh`找到一个文件夹比较大的挂载点,把`dataDir`和`dataLogDir`放在该目录里面
* 集群模式下,每个服务器都需要执行以上操作,注意`每个myid文件和server.ID中的ID一致`
* 复制文件的命令:`scp -P 51668 apache-zookeeper-3.6.2-bin.tar.gz bbd@10.28.200.234:/home/bbd/hulin`

## 修改环境变量
1. 此时进入`"%ZK_HOME%"/bin`目录执行启动脚本`sudo sh zkServer.sh start`，如果出现如下报错
```bash
[bbd@shucang01 bin]$ sudo sh zkServer.sh start
Error: JAVA_HOME is not set and java could not be found in PATH.
```
2. 执行`export`命令，找到java_home
![JAVA_HOME](export.png)

3. 在`"%ZK_HOME%"/conf`目录下找到`zkEnv.sh`，windows是zkEnv.cmd，添加如下内容
`export JAVA_HOME=/usr/local/src/jdk1.8.0_112`

![环境变量](env.png)

## 启动服务
* 在`"%ZK_HOME%"/bin`目录下有一个`zkServer.sh`的启动脚本, 执行命令`sudo sh zkServer.sh start`，当回显如下时，表示启动成功，如果启动失败，在`"%ZK_HOME%"/logs`下查看日志的打印，里面会有失败的原因，多半都是上面的配置没有写对，仔细检查一下即可
``` bash
[bbd@shucang01 bin]$ sudo sh zkServer.sh start
ZooKeeper JMX enabled by default
Using config: /home/bbd/hulin/apache-zookeeper-3.6.2-bin/bin/../conf/zoo.cfg
Starting zookeeper ... STARTED
```

## 验证服务
1. 输入命令`telnet 127.0.0.1 2181`，然后输入`stat`:
``` bash
[bbd@shucang01 bin]$ telnet 127.0.0.1 2181
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
stat
stat is not executed because it is not in the whitelist.
Connection closed by foreign host.
```
2. 如果回显报错：stat is not executed because it is not in the whitelist，解决办法就是在zoo.cfg文件中增加如下命令，然后重启服务即可
``` bash
#开启四字命令
4lw.commands.whitelist=*
```

3. 当出现如下回显时，证明问题解决了
``` bash
[bbd@shucang03 bin]$ telnet 127.0.0.1 2181
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
stat
Zookeeper version: 3.6.2--803c7f1a12f85978cb049af5e4ef23bd8b688715, built on 09/04/2020 12:44 GMT
Clients:
/127.0.0.1:16149[0](queued=0,recved=1,sent=0)
Latency min/avg/max: 0/0.0/0
Received: 1
Sent: 0
Connections: 1
Outstanding: 0
Zxid: 0x200000000
Mode: follower
Node count: 5
Connection closed by foreign host.
[bbd@shucang03 bin]$
```

