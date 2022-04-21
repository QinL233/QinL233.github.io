---
title: 'springcloud alibaba seata'
layout: post
tags:
  - java
category: java
---
springcloud alibaba seata- 微服务分布式事务框架。

<!--more-->

# 一、docker部署


## 配置文件

file.conf

```
## transaction log store, only used in seata-server
store {
  ## store mode: file、db、redis
  mode = "db" ## 原来为file
 
  ## file store property
  file {
    ## store location dir
    dir = "sessionStore"
    # branch session size , if exceeded first try compress lockkey, still exceeded throws exceptions
    maxBranchSessionSize = 16384
    # globe session size , if exceeded throws exceptions
    maxGlobalSessionSize = 512
    # file buffer size , if exceeded allocate new buffer
    fileWriteBufferCacheSize = 16384
    # when recover batch read size
    sessionReloadReadSize = 100
    # async, sync
    flushDiskMode = async
  }

  ## database store property
  db {
    ## the implement of javax.sql.DataSource, such as DruidDataSource(druid)/BasicDataSource(dbcp)/HikariDataSource(hikari) etc.
    datasource = "druid"
    ## mysql/oracle/postgresql/h2/oceanbase etc.
    dbType = "mysql"
    driverClassName = "com.mysql.jdbc.Driver"
    ## 因为设置为db，所以需要选择数据库，这里设置数据库及密码，seata是需要创建的，默认的表是通过脚本运行得到的
    url = "jdbc:mysql://192.168.1.23:3306/seata"
    user = "root"
    password = "123456"
    minConn = 5
    maxConn = 30
    globalTable = "global_table"
    branchTable = "branch_table"
    lockTable = "lock_table"
    queryLimit = 100
    maxWait = 5000
  }

  ## redis store property
  redis {
    host = "127.0.0.1"
    port = "6379"
    password = ""
    database = "0"
    minConn = 1
    maxConn = 10
    queryLimit = 100
  }

```

registry.conf
```
registry {
  # file 、nacos 、eureka、redis、zk、consul、etcd3、sofa
  ## 我们这里使用nacos作为注册中心，所以需要设置type为nacos并设置nacos的属性
  type = "nacos"
  nacos {
    application = "seata-server"
    serverAddr = "192.168.1.23:8848"
    group = "SEATA_GROUP"
    ## 这里的namespace是你nacos服务的namespace的data-id，设置那里，到时就会在那个namespace中
    namespace = "a1493ff3-a94b-4f76-91af-7999fabff7d5"
    cluster = "default"
    username = "nacos"
    password = "nacos"
  }
}

config {
  # file、nacos 、apollo、zk、consul、etcd3
  type = "file"

  file {
    name = "file.conf"
  }
}

```

## 启动容器

```shell
docker run -d --restart always --name  seata-server -p 8091:8091  -v /usr/local/seata-1.3.0:/seata-server -e SEATA_IP=192.168.1.23 -e SEATA_PORT=8091 seataio/seata-server:latest

```

--privileged=true	使得容器内的root拥有真正外部root的权限

--restart=always	是的容器在docker启动时自动启动
