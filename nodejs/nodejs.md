# nodejs学习笔记
## CenterOS环境下安装nodejs
- 方法一（失败）
```shell
#注备命令
yum -y install gcc make gcc-c++ openssl-devel wget
#下载解压
wget http://nodejs.org/dist/v0.10.26/node-v0.10.26.tar.gz
tar -zvxf node-v0.10.26.tar.gz
#安装编译
./configure
make
make install
```
- 方法二（成功）
```shell
#如何从EPEL库安装Node.js
[root@dev-41 nodejs]# yum install epel-release
#下面的命令报错了
[root@dev-41 nodejs]# yum install nodejs
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
#通过修改下面文件，将
#baseurl
mirrorlist
改为
baseurl
#mirrorlist
[root@dev-41 nodejs]# vi /etc/yum.repos.d/epel.repo
[root@dev-41 nodejs]# yum install npm

```

## npm 常见命令

+ npm install moduleName //安装包,也可以简写 npm i moduleName
``` nodejs
直接使用npm install 命令会更具package.json文件的dependencies项的依赖来安装
-g 安装到全局，代码中不能通过require获取，一般用作命令行操作 如 npm intall -g forever
npm i node_module -save //自动更新dependencies字段
npm i node_module -save-dev //自动更新devDependencies字段
```
+ npm list //参看当前目录安装包
+ npm help forever //参看forever命令的帮助信息
+ npm view forever dependencies //查看forever包的依赖关系
+ npm view forever repository.url //查看forever包源文件地址
+ npm outdated //查看所有npm所有过期的包，需要更新
+ npm update moduleName //更新包
+ npm uninstall moduleName //卸载包
+ npm search moduleName //查看包是否存在
+ npm init //引导创建package.json文件
+ npm root //查看当前包安装的目录
``` nodejs
npm root -g //查看包全局安装的目录
```
+ npm -v //查看npm版本
+ npm config set registry https://registry.npm.taobao.org // 永久使用镜像
+ npm --registry "http://npm.hacknodejs.com/" install underscore // 临时使用镜像

## 常见问题
##### npm start命令出现一下错误
```shell 
Failed to load c++ bson extension, using pure JS version
```
解决方法：    
<code>MAC</code>系统运行<code>xcode-select --install</code>，然后运行    
```shell
rm -rf node_modules
npm cache clean
npm install
```
参考： http://stackoverflow.com/questions/21656420/failed-to-load-c-bson-extension    
 
