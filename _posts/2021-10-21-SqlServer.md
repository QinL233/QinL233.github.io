---
title: 'Sql server'
layout: post
tags:
  - Sql server
category: Sql server
---
Sql server.

<!--more-->

# 一、docker安装

```shell
docker pull exoplatform/sqlserver

docker run -d -e SA_PASSWORD='dalong!@123' -e SQLSERVER_DATABASE=demo -e SQLSERVER_USER=dalong -e SQLSERVER_PASSWORD='dalong!@123' -p 1444:1433 exoplatform/sqlserver
```

