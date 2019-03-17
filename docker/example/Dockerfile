FROM centos:7
LABEL maintainer="username <username@email.com>"
LABEL name="CentOS Base Image For FE"



# 新建软件文件
RUN mkdir /soft && cd /soft \
&& yum clean all \
&& yum install deltarpm -y \
# 系统软件更新
&& yum update -y \
# 安装 wget/nginx/scp/zip/unzip/bzip2/sshd/witch/sudo
&& yum install wget nginx openssh-server zip unzip bzip2 openssl-server which sudo -y \
# 安装git及相关库
&& yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel -y \
# 安装 make gcc，node-sass需要
&& yum install make gcc* -y \
# 安卓nvm
&& curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash \
# 初始化命令
&& source ~/.bash_profile \
&& source ~/.bashrc \
# 安装制定node版本
&& export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node \
&& export SQLITE3_BINARY_SITE=http://npm.taobao.org/mirrors/sqlite3 \
&& nvm install v8.9.0 \
&& nvm use v8.9.0 \
&& nvm cache clear \
&& npm i yarn pm2 -g \
# 输出环境变量
&& echo -e "\
phantomjs_cdnurl=https://npm.taobao.org/mirrors/phantomjs/\n\
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/\n\
electron_mirror=https://npm.taobao.org/mirrors/electron/\n\
chromedriver_cdnurl=http://npm.taobao.org/mirrors/chromedriver/\n\
operadriver_cdnurl=http://npm.taobao.org/mirrors/operadriver/\n\
profiler_binary_host_mirror=http://npm.taobao.org/mirrors/node-inspector/\n\
npm_config_profiler_binary_host_mirror=http://npm.taobao.org/mirrors/node-inspector/\n\
registry=https://registry.npm.taobao.org\
" >> ~/.npmrc \
# 生成ssh key
&& ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key \
&& ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key \
&& ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key \
&& ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key \
# nginx配置
&& mkdir -p /workspace/nginx \
&& mkdir -p /workspace/logs \
# 清理缓存
&& rm -rf /tmp/* /var/cache/yum/*

COPY nginx.conf /etc/nginx/.


# 设置系统环境变量
# ENV NODE_ENV=development WORKSPACE="/workspace"

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
