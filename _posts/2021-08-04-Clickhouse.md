---
title: 'OLAP数据库 Clickhouse'
layout: post
tags:
  - ntpdate 
category: Centos
---
OLAP数据库 Clickhouse 安装和使用。

<!--more-->

# 一、docker安装

```shell
docker pull yandex/clickhouse-server

docker run -d --name clickhouse-server --ulimit nofile=262144:262144 -p 8123:8123 -p 9000:9000 -p 9009:9009 yandex/clickhouse-server

```

