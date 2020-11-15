---
title: 一天一个linux命令——du
top: false
cover: false
toc: true
mathjax: true
date: 2020-11-15 14:44:30
password:
summary: du命令详解
tags:
- linux
- 磁盘
categories:
- linux
---

# du命令详解

# 常用命令

## 查看文件夹大小
``` bash
[bbd@shucang01 ~]$ du -h -d 1
1.8G	./liudequan
4.0K	./.oracle_jre_usage
16K	./.ssh
0	./.beeline
32M	./hulin
0	./.pki
440M	./tools
307M	./software
4.0K	./.vim
2.5G	.

```

## 查看文件夹所属分区
``` bash
[bbd@shucang01 data1]$ df /home
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sda5       86987268 8263112  78724156  10% /
```

## 查看文件系统分区情况
``` bash
[bbd@shucang01 data1]$ df -lh
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda5        83G  7.9G   76G  10% /
devtmpfs        3.9G     0  3.9G   0% /dev
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           3.9G   34M  3.8G   1% /run
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/sda1       509M  128M  382M  26% /boot
/dev/sda2       512M   16K  512M   1% /boot/efi
tmpfs           783M     0  783M   0% /run/user/0
/dev/sdb        500G   33M  500G   1% /data1
tmpfs           783M     0  783M   0% /run/user/1001
```
