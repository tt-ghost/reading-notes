# Docker基础概要

## MAC安装

```shell
brew cask install docker
```

## 获取镜像

docker pull [选项] [仓库地址[:端口]/]仓库名[:标签]

```shell
docker pull ubuntu:16.04
```

## 进入docker 

```shell
# i为交互模式，t为终端；进入ubuntu系统并启用bash命令
docker run -it ubuntu bash
# 启动一个 webserver
docker run -d -p 80:80 --name webserver nginx
docker stop webserver
docker rm webserver
# 执行webserver 的nginx
docker exec -it webserver nginx
```

## 退出docker

```shell
exit
```

## 列出本地镜像

```shell
docker images
# 或者
docker iamge ls
```

## 删除本地镜像

```shell
# docker image rm [选项] <镜像> [<镜像2>...]
# <镜像> 为镜像ID
```

## 比较docker修改的变化详情

```shell
docker diff webserver
```

## 讲修改内容保存起来 `docker commit` **一定慎用**，通常需要修改`Dockerfile`文件进行定制化镜像

```shell
# docker commit [选项] <容器名或ID> [仓库名[:标签]]
docker commit --author 'tt <ttghost@126.com>' —message '修改网页' webserver nginx:v2
# --author 制定修改的作者
```

## 具体查看镜像内的历史记录

```shell
docker history nginx:v2
```

## 运行定制好的镜像

```shell
docker run —name web2 -d -p 81:80 nginx:v2
# 本地访问 81 端口即可打开 
```

## 定制镜像，通过Dockerfile配置

```shell
FROM nginx
RUN echo 'test info'
```

## 特殊镜像 `scratch`，表示空白镜像，如果已此镜像为基础镜像，将不以任何镜像为基础。接下来的指令将作为第一层开始存在，常见如 `swarm` 、`coreos/etcd`，使用 `go` 语言开发很多用此镜像作为基础

```shell
FROM scratch
# ...
```

## RUN 执行命令，用来执行命令行命令的

```shell
# RUN [可执行文件, 参数1， 参数2]
RUN echo 'test' > /usr/webroot/index.html
```

### 示例1

```shell
FROM debian:jessie

RUN apt-get update
RUN apt-get install -y gcc libc6-dev make
RUN wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz"
RUN mkdir -p /usr/src/redis
RUN tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1
RUN make -C /usr/src/redis
RUN make -C /usr/src/redis install
```

- Dockerfile 中的每一个指令都会建立一层，以上相当于常见了 7 层镜像，显然不是很好
- Union FS 曾最大不得超过42层，现在不超过 127 层

**修改以上脚本如下**

```shell
FROM debian:jessie

RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
```

## 构建镜像 `docker build`，此命令并非在本地构建，而是服务端，即Docker引擎中构建

```shell
# docker build [选项] <上下文路径/URL/->
docker build -t nginx:v3 .
# . 为当前执行上下文
```

## `COPY` 命令

```shell
# ./package.json 存放在执行 COPY 命令上下文目录中，并非 docker build命令所在目录，也不是Dockerfile所在目录
COPY ./package.json /app/
# COPY ../package.json /APP 或 COPY /opt/xxx /app 会报错，因为超出上下文范围；
# 如果需要的话，需要复制到 COPY 的上下文目录内
```

注意事项：
- `Dockerfile` 一般存放在项目根目录或空目录，会当前目录构建到镜像中，如果需要排除，可以配置 `.dockerignore` 文件
- `Dockerfile` 可以是别的名字，并且可以在项目外面，如制定 `-f` 参数：
```shell
-f ../Dockerfile.php
```

## 基于 `git repo` 进行构建

```shell
docker build http://github.com/twang2218/gitlib-ce-zh.git#:8.14
# ...
```

此命令指定了构建所需的git ，并指定默认分支`master`，构建目录 `/8.14`

## 基于`tar` 包构建

```shell
docker build http://server/context.tar.gz
# Docker 引擎会自动下载并解压然后构建
```

## 从标准输入中构建，没有上下文，不能将本地文件 `COPY` 进镜像内

```shell
docker build - < Dockerfile
# 或
cat Dockerfile | docker build -
# 基于输入的压缩包构建，发现 gzip、bzip2、xz类型的文件，自动展开，将内容视为上下文并构建
docker build - < context.tar.gz

```
