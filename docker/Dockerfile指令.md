# Dockerfile指令

## `COPY` 复制文件，第一种命令行方式，第二种编程方式

> shell格式：COPY <源路径> ... <目标路径>
> exec格式：COPY ["<源路径>" ... "<目标路径>"]

- 路径可以是多个，也可以通配符
```dockerfile
COPY hom* /test/
```
- 使用 `COPY` 会保留源文件的元数据，读、写、执行、变更时间等，同时目标路径可以不存在，在复制之前，不存在的话会自动创建

## `ADD` 更高级的复制文件，除了含有 `COPY`的命令，还会对 `gzip`、`bzip2`、`xz`等文件自动解压

## `CMD` 容器启动命令，与 `RUN` 命令相似

> CMD <命令>
> CMD ["可执行文件", "参数1", ...]

- Docker 不是虚拟机，容器就是进程。
- `CMD` 指令就是用于指定默认的容器主进程启动命令的
- `ubuntu`镜像默认`CMD`指向`/bin/bash`，如 `docker run -it ubuntu`会进入`bash` 
- 如果希望指定主进程，可以：
```shell
docker run -it ubuntu cat /etc/os-release
```
- `exec`方式一定要用双引号，如果使用 `shell` 方式，实际的命令被包装为`sh -c` 的参数形式执行，如：
```dockerfile
CMD echo $HOME
# 实际将变为
CMD ["sh", "-c", "echo $HOME"]
```
- 容器中的应用都应该以前台执行，容器中没有后台的概念
```shell
# 容器执行后会立即退出
CMD service nginx start
# 相当于以下，主进程为 sh，当 service nginx start 结束，sh作为主进程也就结束
CMD ["sh", "-c", "service nginx start"]
# 正确方式
CMD ["nginx", "-g", "daemon off;"]
```

## `ENTRYPOINT` 入口点，与`RUN`命令格式一样，用途与`CMD`一样，都是在制定容器启动程序及参数

当指定了`ENTRYPOINT`后，`CMD`不在是直接的运行命令，而是将`CMD`的内容作为参数传给`ENTRYPOINT`指令

## `ENV` 设置环境变量

格式：
```shell
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2> ...
ENV VERSION=1.0 NAME="new name"
```

## `ARG` 构建参数

与`ENV` 效果一样，设置环境变量，，`ARG`构建的变量在容器运行时不会存在这些变量，但是可以通过 `docker history` 查看所有值
格式：
```shell
ARG <参数名>[=<默认值>]
```

## `VOLUME`定义匿名卷

为了将容器存储层的无状态化，可以挂载本地任意目录为匿名卷，任何向卷中写入的数据都不会进入容器的存储层

格式：
```dockerfile
VOLUME ["<路径1>", "<路径2>" ...]
# 或者
VOLUME <路径>

VOLUME /data
```

```shell
# 使用 mydata 这个命名卷挂载到了 /data 这个位置
docker run -d -v mydata:/data xxxx
```

## `EXPOSE` 声明端口

格式：`EXPOSE <端口1> [<端口2> ...]`

声明运行时容器提供的服务端口，只是一个声明，运行时并不一定会开启这个端口服务。`EXPOSE`与在运行时使用 `-p <宿主端口>:<容器端口>` 不同，`-p` 是映射宿主端口和容器端口，而 `EXPOSE`仅仅是声明，不会自动在宿主进行端口映射


## `WORKDIR`指定工作目录

格式：`WORKDIR <工作目录路径>`

如果不存在会自动创建。`Dockerfile`不能等同于shell脚本，如下命令可能在 `/app/world.txt` 中找不到文件

```shell
RUN cd /app
RUN echo "hello" > world.txt
```

原因：shell 的连续两行在同一个进程执行环境，而`Dockerfile` 中，两行`RUN` 执行环境不同，第一层只是改变了内存，而第二层是全新的容器，跟第一层完全没关系

## `USER` 指定当前用户

格式: `USER <用户名>`

与`WORKDIR` 类似，改变环境状态并影响以后的层，使用前需要有此用户或创建

```shell
RUN groupadd -r redis && useradd -r -g radis radis
USER radis
RUN ["radis-server"]
```

如果以`root` 身份运行，不要使用`su` 或 `sudo` ，配置比较麻烦，建议使用 [gosu](https://github.com/tianon/gosu)

```dockerfile
# 建立 redis 用户，并使用 gosu 换另一个用户执行命令
RUN groupadd -r redis && useradd -r -g redis redis
# 下载 gosu
RUN wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true
# 设置 CMD，并以另外的用户执行
CMD [ "exec", "gosu", "redis", "redis-server" ]
```

## `HEALTHCHECK`健康检查

格式：
- `HEALTHCHECK [选项] CMD <命令>` : 设置检查容器健康状况的命令
- `HEALTHCHECK NONE`: 如果基础镜像有健康检查指令，这里可以屏蔽掉其健康检查指令


## `ONBUILD` 为他人做嫁衣

格式：`ONBUILD <其他指令>`

后面跟其他指令，在当前镜像构建时不会执行，只有当以当前镜像作为基础镜像，去构建下一级镜像的时候才会执行

