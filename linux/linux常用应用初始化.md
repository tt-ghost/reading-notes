# linux常用应用初始化

## 安装wget

```shell
yum install wget -y
```

## 增加系统软件仓库

```shell
mkdir /soft && cd /soft
rpm -Uvh http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
# 查看系统仓库列表
yum repolist
# 卸载
# rpm -e epel-release
```

## 安装nginx\scp\nodejs\zip\unzip\sshd\which

```shell
yum install nginx openssh-server nodejs zip unzip openssl-server which -y
```

## 安装 git

```shell
yum install curl-devel expat-devel gettext-devel   openssl-devel zlib-devel -y
```

## 安装 nvm

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
source ~/.bash_profile
source ~/.bashrc
nvm -v
```

