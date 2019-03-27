## phantomjs使用笔记

### 安装
- 下载地址    
推荐使用淘宝镜像地址：    
http://npm.taobao.org/dist/phantomjs/   
对应MAC版：    
http://oss.npm.taobao.org/dist/phantomjs/phantomjs-2.0.0-macosx.zip    
linux只有源码：    
http://oss.npm.taobao.org/dist/phantomjs/phantomjs-2.0.0-source.zip 
    

-  添加环境变量
mac上：   
```shell
#复制到系统资源目录
localhost:test fish$ sudo cp -r phantomjs-2.0.0-macosx /usr/local/src/.
#添加系统变量
localhost:test fish$ sudo ln -s /usr/local/src/phantomjs-2.0.0-macosx/bin/phantomjs /usr/local/bin/phantomjs
#出错了
localhost:test fish$ phantomjs -v
Killed: 9
```
**解决方法**：    
1 先安装upx
```shell 
$ brew install upx
```
2 执行
```shell
$ sudo upx -d phantomjs-2.0.0-macosx/bin/phantomjs
```
3 运行
```shell
$ phantomjs
phantomjs> 
```

参考：http://stackoverflow.com/questions/28267809/phantomjs-getting-killed-9-for-anything-im-trying 


