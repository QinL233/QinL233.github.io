---
title: 'Google Maglev 简介'
layout: post
tags:
  - Install
category: Docker
---
Docker 安装过程。

<!--more-->

# 一、编译环境

```shell
yum -y install gcc
yum -y install gcc-c++
```



# 二、卸载旧版本

```shell
yum remove docker \ docker-client \ docker-client-latest \ docker-common \ docker-latest \ docker-latest-logrotate \ docker-logrotate \ docker-selinux \ docker-engine-selinux \ docker-engine
```



# 三、安装依赖包

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```



# 四、设置stable镜像仓库

```shell
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

或者

```shell
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



# 五、更新yum软件包索引

```shell
yum makecache fast
```



# 六、安装

查看版本

```shell
yum list docker-ce.x86_64  --showduplicates | sort -r
```

安装指定版本

```shell
yum -y install docker-ce-18.03.1.ce-1.el7.centos docker-ce-cli-18.03.1.ce-1.el7.centos containerd.io
```



# 七、启动并设置开机自启

```shell
systemctl start docker
systemctl enable docker
```

