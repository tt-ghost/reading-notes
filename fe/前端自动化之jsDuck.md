# 前端自动化之jsDuck
  
### 安装
- 首先确认有没有安装ruby环境
```shell
#ubuntu安装：
sudo apt-get install ruby
#CenterOS安装：
sudo yum install ruby
```
###安装过程  

###### 使用gem install jsduck进行安装
```
fishdeMacBook-Pro:jsduck fish$ gem install jsduck
ERROR:  Could not find a valid gem 'jsduck' (>= 0), here is why:
          Unable to download data from https://rubygems.org/ - Errno::ECONNRESET: Connection reset by peer - SSL_connect (https://rubygems.org/latest_specs.4.8.gz)
```

###### 将http://rubygems.org 作为gem源
```
fishdeMacBook-Pro:jsduck fish$ sudo gem sources -a http://rubygems.org
Password:
https://rubygems.org is recommended for security over http://rubygems.org
Do you want to add this insecure source? [yn]  y
http://rubygems.org added to sources
```

###### 如果网络不好，可该用淘宝的源
```
[root@dev-41 jsduck]# gem sources -a http://ruby.taobao.org
http://ruby.taobao.org added to sources
[root@dev-41 jsduck]# gem sources
*** CURRENT SOURCES ***

http://rubygems.org
http://ruby.taobao.org
```

###### 同时删除官方的源
```
[root@dev-41 jsduck]# gem sources -r https://rubygems.org
```

###### 重新执行安装命令，发现依然有问题
```
fishdeMacBook-Pro:jsduck fish$ gem install jsduck
Building native extensions.  This could take a while...
ERROR:  Error installing jsduck:
	ERROR: Failed to build gem native extension.

/usr/bin/ruby extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/ruby.h


Gem files will remain installed in /usr/lib/ruby/gems/1.8/gems/rdiscount-2.1.8 for inspection.
Results logged to /usr/lib/ruby/gems/1.8/gems/rdiscount-2.1.8/ext/gem_make.out
```

###### 打开/usr/lib/ruby/gems/1.8/gems/rdiscount-2.1.8/ext/gem_make.out文件
```
[root@dev-41 jsduck]# vi /usr/lib/ruby/gems/1.8/gems/rdiscount-2.1.8/ext/gem_make.out
```

###### 以下为/usr/lib/ruby/gems/1.8/gems/rdiscount-2.1.8/ext/gem_make.out文件内容
```
/usr/bin/ruby extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/ruby.h
```

###### 解决方案
```
[root@dev-41 jsduck]# yum install ruby-devel
...
```

###### 正确安装
```
[root@dev-41 jsduck]# gem install jsduck
```

参考连接：  
[http://stackoverflow.com/questions/19150017/ssl-error-when-installing-rubygems-unable-to-pull-data-from-https-rubygems-o](http://stackoverflow.com/questions/19150017/ssl-error-when-installing-rubygems-unable-to-pull-data-from-https-rubygems-o)

### 使用
- 运行
```shell
#将test/src目录下文件转成doc文档到docs目录
jsduck test/src --output docs


```
