---
title: docker基本操作
top: false
cover: false
toc: true
mathjax: true
date: 2020-08-25 20:55:35
password:
summary: docker的常用命令
tags:
- docker
categories:
- docker
---

本教程基于CentOS7

## 安装docker

### 使用yum安装

1. 升级yum

    ```bash
    sudo yum update
    ```

2. 安装社区版

    ```bash
    sudo yum install docker-ce
    ```

3. 查看安装是否成功

    ``` bash
    sudo docker -v
    ```

### 使用脚本安装

```bash
任意执行以下一个命令即可
$ curl -fsSL https://get.docker.com/ | sudo sh

$ sudo wget -qO- https://get.docker.com/ | bash

$ curl -fsSL https://get.docker.com -o get-docker.sh
```

## 设置USTC镜像

加快docker镜像的下载速度
编辑文件

```bash
vi /etc/docker/daemon.json
```

输入以下内容

```bash
{
    "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

## docker启动与停止

systemctl 是系统服务管理器指令

1. 开机启动

    ```bash
    sudo systemctl enable docker
    ```

2. 停止

    ```bash
    sudo systemctl stop docker
    ```

3. 启动docker

    ```bash
    sudo systemctl statr docker
    ```

4. 重启

    ```bash
    sudo systemctl restart docker
    ```

5. 查看docker状态

    ```bash
    sudo systemctl status docker
    sudo docker info
    ```

6. docker帮助

    ```bash
    sudo docker --help
    ```

## 镜像相关命令

1. 查看镜像

    ``` bash
    sudo docker images
    ```

2. 搜索镜像

    ``` bash
    sudo docker search [image_name]
    ```

3. 拉取镜像

    ``` bash
    sudo docker pull [image_name:version]
    ```

    如果没有版本号，默认拉取latest镜像

4. 删除镜像

    ``` bash
    sudo docker rmi [image_id]
    ```

5. 删除所有镜像

    ``` bash
    sudo docker rmi `docker images -q`
    ```

## 容器相关命令

1. 查看容器

    ``` bash
    sudo docker ps
    ```

2. 查看所有容器

    ``` bash
    sudo docker ps -a
    ```

3. 查看指定容器

    ``` bash
    sudo docker ps | grep xxx
    ```

4. 查看最后一次运行的容器

    ``` bash
    sudo docker ps -l
    ```

5. 查看停止的所有容器

    ``` bash
    sudo docker ps -f status=exited`
    ```

6. 进入容器

    ``` bash
    sudo docker exec -it [container_name] /bin/bash ## 方法1
    sudo docker attach [container_name] ## 方法2(这个不推荐使用，因为每次退出终端，container就停止运行了)
    ```

7. 退出容器

    ``` bash
    CTRL + D ## 方法1
    输入exit ## 方法2
    ```

8. 停止容器

    ``` bash
    sudo docker stop [container_id/container_name]
    ```

9. 启动容器

    ``` bash
    sudo docker statr [container_id/container_name]
    ```

10. 容器与宿主机之间的文件拷贝

    ``` bash
    sudo docker cp [本机路径] [容器名称:路径] ## 拷入
    sudo docker cp [容器名称:路径] [本机路径] ## 拷出
    ```

11. 创建并启动新的容器
    {% post_link docker-jenkins-持续集成 点击这里查看这篇文章 %}

12. 查看容器信息

    ``` bash
    输出容器的所有信息
    sudo docker inspect [container_id/container_name]

    查看其中指定的信息
    sudo docker insperct --format='{{.NetWorkSettings.IPAddress}}' [container_id/container_name]
    ```

13. 删除容器

    ``` bash
    删除之前需要先停止运行的容器
    sudo docker rm [container_id/container_name]
    ```

## 备份与恢复

1. 把container打包成image

    ``` bash
    sudo docker commit [container_name] [new_image_name]
    ```

2. 把image保存为文件，方便备份和迁移

    ``` bash
    sudo docker save -o [file_name.tar] [image_name]
    ```

3. 文件恢复成镜像

    ``` bash
    sudo docker load -i [file_name.tar]
    ```
