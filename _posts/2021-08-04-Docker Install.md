---
title: 'Docker Install'
layout: post
tags:
  - docker
category: docker
---
docker安装过程。

<!--more-->

# 一、编译环境

```shell
yum -y install gcc gcc-c++

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

# 八、配置新的网卡避免路由冲突
关闭docker
vim /etc/docker/daemon.json

```
{
  "bip": "192.168.0.1/16",
  "registry-mirrors": [
    "https://registry.aliyuncs.com"
  ]
}
```



重启验证
```
systemctl daemon-reload

systemctl restart docker

ifconfig

route -n
```


# 九、配置新的存储未知避免空间不足

```
#创建新路径
mkdir -p /home/docker/lib/

#迁移数据
rsync  -avz  /var/lib/docker/ /home/docker/lib/

```

创建配置文件
vim /etc/docker/daemon.json

```
{
  "bip": "192.168.0.1/16",
  "data-root": "/home/docker/lib",
  "registry-mirrors": [
    "https://registry.aliyuncs.com"
  ]
}
```


重启并验证
```
systemctl  daemon-reload

systemctl  restart  docker

docker info
```