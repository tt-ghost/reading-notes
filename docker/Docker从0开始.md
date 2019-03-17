# Docker从0开始

```shell
# 登录镜像仓库
# docker login -u <username> -p <password> <server>
# 或者 docker login <server>
docker login hub.xxxx.com

# 退出
docker logout

# 查看docker 配置信息
docker info

# 远端拉取镜像
docker pull centos:7.2.1511

# 临时进入镜像
docker run -it centos:7.2.1511 bash
# -d 参数为放在后台运行
# docker run -d centos:7.2.1511 bash

# 此处配置Dockerfile

# 通过Dockerfile构建镜像， -t 为 tag
docker build -t mydocker:01 .

# 通过参数 -f 指定的Dockerfile进行构建，这里文件为 Docker.fe
docker build -t mydocker:01 . -f Docker.fe

# 打tag

docker tag mydocker:01 hub.xxx.com/username/mydocker:01

# 上传镜像
# docker push mydocker:01
docker push hub.xxx.com/username/mydocker:01

# 创建容器
# doceker create -it --name centos_fe mydocker:01 bash
doceker create --name centos_fe mydocker:01

# 启动容器
docker start <container id>

# 以交互方式进入
docker exec -it <container id> bash
```

## 镜像操作

```shell
# 查看本地镜像
docker images
# 或者
docker image ls

# 删除本地镜像
docker image rm <image id>

# 强制删除
docker image rm <image id>

# 上传镜像
docker push

```

## 容器操作

```shell
# 创建容器
docker create <image id>
# 或者
doceker create -it --name mydocker_container centos:7.2.1511 bash
# 或者
doceker create -it --name mydocker_container <image id> bash
# 推荐：或者不用 -it 参数，容器命令将为Docker 中的CMD命令
doceker create --name mydocker_container <image id>

# 查看已经启动的容器
docker ps

# 查看所有容器
docker ps -a

# 启动容器
docker start <container id>

# 停止容器
docker stop <container id>

# 删除容器
docker rm <container id>

# 以交互方式进入容器中
docker exec -it <container id> bash
# 或者
docker exec -it mydocker_container bash

# 将容器的存储层保存先来成为镜像，即在原有镜像的基础上，再叠加了一层容器的存储层，构成新的镜像
docker commit [选项] <container id 或 container name> [仓库名[:标签]]
# 如
docker commit --author "username <username@email.com>" --message "修改用户信息" mydocker_container

```

## Dockerfile操作

```dockerfile
FROM centos:7
# 或者
# FROM hub.xxx.com/username/centos:7.2.1511

LABEL maintainer="username <username@email.com>"

RUN yum install openssh-server -y
# 其他工具安装

EXPOSE 22
# 守护进程
CMD ["/usr/sbin/sshd", "-D"]
```
