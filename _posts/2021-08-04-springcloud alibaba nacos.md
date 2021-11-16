---
title: 'springcloud alibaba nacos'
layout: post
tags:
  - java
category: java
---
springcloud alibaba nacos - 微服务服务发现框架。

<!--more-->

# 一、docker安装

## mysql脚本

下载地址：https://github.com/alibaba/nacos/blob/master/config/src/main/resources/META-INF/nacos-db.sql

## 配置文件

custom.properties

```
server.contextPath=/nacos
server.servlet.contextPath=/nacos
server.port=8848
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://192.168.101.136:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=root
nacos.cmdb.dumpTaskInterval=3600
nacos.cmdb.eventTaskInterval=10
nacos.cmdb.labelTaskInterval=300
nacos.cmdb.loadDataAtStart=false
management.metrics.export.elastic.enabled=false
management.metrics.export.influx.enabled=false
server.tomcat.accesslog.enabled=true
server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i
nacos.security.ignore.urls=/,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/login,/v1/console/health/**,/v1/cs/**,/v1/ns/**,/v1/cmdb/**,/actuator/**,/v1/console/server/**
nacos.naming.distro.taskDispatchThreadCount=1
nacos.naming.distro.taskDispatchPeriod=200
nacos.naming.distro.batchSyncKeyCount=1000
nacos.naming.distro.initDataRatio=0.9
nacos.naming.distro.syncRetryDelay=5000
nacos.naming.data.warmup=true
nacos.naming.expireInstance=true
```

## 创建容器

```shell
docker pull nacos/nacos-server

docker  run --name nacos -d -p 8848:8848 --privileged=true --restart=always -e JVM_XMS=256m -e JVM_XMX=256m -e MODE=standalone -e PREFER_HOST_MODE=hostname -v /home/nacos/logs:/home/nacos/logs -v /home/nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties nacos/nacos-server
```

--privileged=true	使得容器内的root拥有真正外部root的权限

--restart=always	是的容器在docker启动时自动启动
