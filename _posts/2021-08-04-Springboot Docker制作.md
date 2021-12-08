---
title: 'Springboot Docker制作'
layout: post
tags:
  - java
category: java
---
Springboot Docker制作java镜像文件。

<!--more-->

# 思路

创建纯java8环境，通过映射磁盘更新app.jar和start.sh，实现便捷docker容器中启动jar

# 准备工作

1.app/app.jar 运行的jar包

2.app/start.sh 容器启动时运行的脚本

3.Dockerfile 构建镜像

4.jdk文件夹

![20210804163921](https://raw.githubusercontent.com/QinL233/QinL233.github.io/master/images/20210804163921.png)

### Dockerfile

```dockerfile
FROM centos:centos7.5.1804

ADD jdk1.8.0_291/ /home/jdk1.8.0_291/

ENV JAVA_HOME /home/jdk1.8.0_291/
ENV JRE_HOME=${JAVA_HOME}/jre
ENV CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
ENV PATH=${JAVA_HOME}/bin:$PATH

VOLUME ["/home/app/"]

WORKDIR /home/app/

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

CMD ["sh","/home/app/start.sh"]
```

### start.sh

```shell
#!/bin/bash
JAVA_OPTS="-Xmx3550m -Xms3550m -Xmn1256m -XX:MetaspaceSize=256M -XX:MaxMetaspaceSize=256M -XX:SurvivorRatio=6 -XX:ParallelGCThreads=2 -XX:+UseConcMarkSweepGC  -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCTimeStamps -XX:+PrintGCCause -XX:+UseGCLogFileRotation  -Xloggc:./log/gc-%t.log -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M"

LOG_FILE=$(date -d today +"console_%Y-%m-%d_%H_%M_%S.log")

java -jar ${JAVA_OPTS} /home/app/app.jar --spring.profiles.active=prod > ./log/${LOG_FILE}

```

# 构建镜像

```shell
docker build -t [imageName:version] -f Dockerfile .
```

# 创建容器

```shell
docker create -it --name [appName] --net=host -v [/mydir]:/home/app  [imageName:version]
```

# 启动容器

```shell
docker start [appName]
```

