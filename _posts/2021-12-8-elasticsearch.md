---
title: '搜索引擎'
layout: post
tags:
  - elasticsearch
category: elasticsearch
---
elasticsearch.

<!--more-->

# 一、docker安装

```shell
#设置max_map_count不能启动es会启动不起来
cat /proc/sys/vm/max_map_count
sysctl -w vm.max_map_count=262144
sysctl -p
#拉取镜像
docker pull elasticsearch:7.7.0

#启动镜像 访问ip:9200
docker run --name elasticsearch -p 9200:9200 -p 9300:9300  -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml -v /mydata/elasticsearch/data:/usr/share/elasticsearch/data -v /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins -d elasticsearch:7.7.0

#其中elasticsearch.yml是挂载的配置文件，data是挂载的数据，plugins是es的插件，如ik，而数据挂载需要权限，需要设置data文件的权限为可读可写,需要下边的指令。
chmod -R 777 要修改的路径
#-e "discovery.type=single-node" 设置为单节点
#-e ES_JAVA_OPTS="-Xms256m -Xmx256m" \ 测试环境下，设置ES的初始内存和最大内存，否则导致过大启动不了ES

docker run --name elasticsearch -d -e ES_JAVA_OPTS="-Xms512m -Xmx512m" -e "discovery.type=single-node" -p 9200:9200 -p 9300:9300 elasticsearch:7.7.0


```

## 安装并配置elasticsearch-head

```shell
#进入容器修改elasticsearch.yml允许跨域
docker exec -it elasticsearch /bin/bash 

vi config/elasticsearch.yml
#最下边增加两行
http.cors.enabled: true 
http.cors.allow-origin: "*"

#重启生效
docker restart elasticsearch

#elasticsearch-head
#拉取镜像
docker pull mobz/elasticsearch-head:5

#创建容器
docker create --name elasticsearch-head -p 9100:9100 mobz/elasticsearch-head:5

#修改vendor.js
docker cp elasticsearch-head:/usr/src/app/_site/vendor.js ./
vim vendor.js
#vim显示行号命令：set number
#6886行改为：contentType:"application/json;charset=UTF-8"
#7573行改为：contentType:"application/json;charset=UTF-8"
docker cp ./vendor.js elasticsearch-head:/usr/src/app/_site/

#启动容器 访问ip:9100
docker start elasticsearch-head
```

# ik分词器

https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.7.0/elasticsearch-analysis-ik-7.7.0.zip

```shell
#将压缩包移动到容器中
docker cp /tmp/elasticsearch-analysis-ik-7.7.0.zip elasticsearch:/usr/share/elasticsearch/plugins

#进入容器
docker exec -it elasticsearch /bin/bash  

#创建目录
mkdir /usr/share/elasticsearch/plugins/ik

#将文件压缩包移动到ik中
mv /usr/share/elasticsearch/plugins/elasticsearch-analysis-ik-7.7.0.zip /usr/share/elasticsearch/plugins/ik

#进入目录
cd /usr/share/elasticsearch/plugins/ik

#解压
unzip elasticsearch-analysis-ik-7.7.0.zip

#删除压缩包
rm -rf elasticsearch-analysis-ik-7.7.0.zip

```

