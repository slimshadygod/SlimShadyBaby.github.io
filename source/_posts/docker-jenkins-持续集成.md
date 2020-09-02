---
title: docker+jenkins 持续集成
top: false
cover: false
toc: true
mathjax: true
date: 2020-08-25 11:54:12
password:
summary: docker和jenkins持续集成教程
tags:
- jenkins
- docker
categories:
---


## Red Hat Docker 安装

### 安装docker

``` bash
$ curl -sSL https://get.daocloud.io/docker | sh
```

### 启动docker

``` bash
# 启动
$ sudo systemctl start docker
# 重启
$ sudo systemctl restart docker
# 重启
$ sudo service docker restart
```

### 验证是否安装成功

``` bash
$ docker --version
Docker version 19.03.12, build 48a66213fe
```

### 添加源

``` bash
$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
Loaded plugins: fastestmirror
adding repo from: http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
grabbing file http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo to /etc/yum.repos.d/docker-ce.repo
repo saved to /etc/yum.repos.d/docker-ce.repo
```

### 配置镜像加速器

1. 网易

    ``` bash
    $ sudo mkdir -p /etc/docker

    $ sudo tee /etc/docker/daemon.json <<-'EOF'
    {
        "registry-mirrors": ["http://hub-mirror.c.163.com"]
    }
    EOF
    ```

    配置完成后记得重启`docker`服务

    ``` bash
    $ sudo systemctl daemon-reload
    $ sudo systemctl restart docker
    ```

2. 阿里云(推荐使用)

    登录 [阿里云](https://cr.console.aliyun.com/cn-hangzhou/instances/repositories)

    选择左侧菜单`镜像加速器`，上面有配置方法和加速器地址

3. daocloud

    ``` bash
    > $ curl -sSL <https://get.daocloud.io/daotools/set_mirror.sh> | sh -s <http://f1361db2.m.daocloud.io>
    ```

4. 测试镜像下载速度的脚本

    ``` bash
    $ curl -sSL <https://cdn.jsdelivr.net/gh/lework/script/shell/docker_hub_speed_test.sh> | bash
    ```

## 安装 jenkins

### 查看jenkins 版本

``` bash
$ sudo docker search jenkins
```

### pull一个镜像

使用官方推荐的镜像\(下载太慢记得配置镜像加速，推荐使用阿里云\)

``` bash
$ sudo docker pull jenkinsci/blueocean
Using default tag: latest
latest: Pulling from jenkinsci/blueocean
df20fa9351a1: Pull complete
1cb481a13af0: Pull complete
f5efbd400588: Pull complete
4d25cfa48d88: Pull complete
1bc71b1e3fbd: Pull complete
0215630c2b8d: Pull complete
b9a8d0388f94: Pull complete
4edf5a184073: Pull complete
2e52212eaba2: Pull complete
f9c1c600b078: Pull complete
123357adf793: Pull complete
058df73922fe: Pull complete
38612d91e32d: Pull complete
cfb97945d6b1: Pull complete
e685a981a3a2: Pull complete
Digest: sha256:f126e425591c697830c17d178e8e209ef6d9fd3c871ceca80dba5d2b1256a291
Status: Downloaded newer image for jenkinsci/blueocean:latest
docker.io/jenkinsci/blueocean:latest
```

### 启动一个jenkins容器

1. 参数简介

    ``` bash
    docker run \
    -u root \
    --name jenkins-blueocean
    --rm \
    -d \
    -p 8080:8080 \
    -p 50000:50000 \
    -v jenkins-data:/var/jenkins_home \
    -v /var/run/docker.sock:/var/run/docker.sock \
    --restart=always \
    jenkinsci/blueocean \
    /bin/bash
    ```

    * `-u`: 创建容器时使用的用户
    * `--name`: Docker容器的名字
    * `--rm`: 退出容器时会删除所有用户数据，慎用
    * `-d`(`--detach`): 在后台运行容器（即“分离”模式）并输出容器ID。如果您不指定此选项， 则在终端窗口中输出正在运行的此容器的Docker日志。
    * `-p`(`--publish`): 第一个,将`jenkinsci/blueocean`容器的端口8080 映射（即“发布”）到主机上的端口8080。第一个数字表示主机上的端口，而最后一个数字表示容器的端口。因此，如果您-p 49000:8080为此选项指定，则将通过端口49000访问主机上的Jenkins。
    * `-p`: 第二个,是有多个jenkins服务时，主备之间通信使用的，单个机器可以不用
    * `-v`: 第一个，将容器的/var/jenkins_home目录，映射到本机的jenkins-data目录（如果没有这个选项，jenkins的历史数据将不会保存，每次启动都是新的），jenkins-data目录如果不存在，会自动创建
    * `--restart=always` 重启docker,container也会自动重启
    * `jenkinsci/blueocean`: jenkins images，如果不存在，会自动下载，所以这个一定要和自己下载的镜像名称一致，不然会下载其他的jenkins images
    * `/bin/bash` 自动进入容器内部

2. 创建实例

    ``` bash
    $ sudo docker run -d --name jenkins_01 -p 8081:8080 -v /home/bbd/tools/jenkins_home:/var/jenkins_01 jenkinsci/blueocean
    3afe63de9b9eebfe486e1417e7071c3e95d9606e2a36521a7d70afa98e4c660d
    ```

3. 查看创建的images

    ``` bash
    $ sudo docker ps | grep jenkins
    3afe63de9b9e        jenkinsci/blueocean   "/sbin/tini -- /usr/…"   About a minute ago   Up About a minute   50000/tcp, 0.0.0.0:8081->8080/tcp   jenkins_01
    ```

4. 访问web页面

    `http://10.28.200.233:8081/`
    需要输入密码，密码查看步骤5

5. 进入images内部,查看密码

    ``` bash
    sudo docker exec -it jenkins_01 bash
    bash-5.0$ cat /var/jenkins_home/secrets/initialAdminPassword
    xxxxxxxxxxxxxxxxxxxx
    ```

6. 其他常用命令

    退出images container： `ctrl + D`

    查看images container id : `sudo docker ps | grep jenkins`

    重启images container: `sudo docker restart ${container_id}`

### 删除镜像

``` bash
$ docker rmi jenkins
```

## 安装jenkins插件

### java项目

1.


## 参考文档

1. [知乎： 史上最全（全平台）docker安装方法](https://zhuanlan.zhihu.com/p/54147784)
2. [jenkins官方文档中文版](https://www.jenkins.io/zh/doc/book/installing/)
3. [jenkins官方文档原版](https://www.jenkins.io/doc/book/installing/)
