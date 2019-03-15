#linux学习笔记之压缩
> 前几天有人问我会不会linux操作，我说会一点，然后问了linux环境下怎么压缩、解压文件，然后、、然后就有点尴尬了。为此觉得有必要把平时学习及工作中常用到的一些命令和掉过的坑总结一下了，一点一点总结。也作为回顾吧

### tar
**常见参数使用 格式 tar -[参数] [目标文件名] [待压缩文件规则]**
+ 主参数：
```
-c #创建  
-r #追加文件到包
-t #查看已经压缩的列表
-u #更新包内的文件
-x #解包
```
+ 辅参数：
```shell
-f #使用归档文件，必选项
-m #解压文件时，把修改文件时间改为现在
-M #创建多卷文件
-p #使用原文件的原有属性，不会因为使用者而改变
-v #查看tar处理的文件信息
-z #使用gzip压缩，gunzip解压，一般文件名为xx.tar.gz或xx.tgz
-j #使用bzip2压缩，unbzip2解压，比gzip压缩更厉害，一般文件名为xx.tar.bz2
-Z #使用compress压缩，uncompress解压，一般文件名为xx.tar.Z

```
+ 实例
```shell
#将当前目录所有文件都打包，当前并没有压缩
fishdeMacBook-Pro:tar fish$ tar -cf tar-cf.tar *  
fishdeMacBook-Pro:tar fish$ ls
cat.txt		cat1.txt	cat2.txt	tar-cf.tar	tarFolder
```
```shell
#查看tar包里的文件信息
fishdeMacBook-Pro:tar fish$ tar -tf tar-cf.tar
cat.txt
cat1.txt
cat2.txt
tarFolder/
tarFolder/tarTest.txt
tarFolder/tarTest2.txt
````

````shell
#查看文件大小，发现打包的文件其实比单独的所有文件大小之和要大
fishdeMacBook-Pro:tar fish$ ls -lh
total 136
-rw-r--r--  1 fish  staff    19K  8 23 18:40 cat.txt
-rw-r--r--  1 fish  staff   349B  8 23 19:45 cat1.txt
-rw-r--r--  1 fish  staff   329B  8 23 19:45 cat2.txt
-rw-r--r--  1 fish  staff    38K  8 23 20:04 tar-cf.tar
drwxr-xr-x  4 fish  staff   136B  8 23 19:47 tarFolder
```

```shell
#使用zip压缩刚才的包
fishdeMacBook-Pro:tar fish$ zip tar-cf.tar.gz tar-cf.tar
  adding: tar-cf.tar (deflated 99%)
```
```shell
#发现压缩率99%，文件从38K到662B
fishdeMacBook-Pro:tar fish$ ls -lh
total 144
-rw-r--r--  1 fish  staff    19K  8 23 18:40 cat.txt
-rw-r--r--  1 fish  staff   349B  8 23 19:45 cat1.txt
-rw-r--r--  1 fish  staff   329B  8 23 19:45 cat2.txt
-rw-r--r--  1 fish  staff    38K  8 23 20:20 tar-cf.tar
-rw-r--r--  1 fish  staff   662B  8 23 20:20 tar-cf.tar.gz
drwxr-xr-x  4 fish  staff   136B  8 23 19:47 tarFolder
```
```shell
#注意tar后面的跟的第一个参数必须为-c, -r, -t, -u, -x或-z, -j, -Z
fishdeMacBook-Pro:tar fish$ tar -fc tar-cf.tar *
tar: Must specify one of -c, -r, -t, -u, -x
```
### zip
**压缩文件**
```shell
#将cat.txt及cat1.txt 压缩为cat.zip，压缩文件夹需要在zip后面加上参数 -r表示递归
fishdeMacBook-Pro:tar fish$ zip cat.zip cat.txt cat1.txt
  adding: cat.txt (deflated 99%)
```
````shell
#解压cat.zip，如果文件存在会提示
fishdeMacBook-Pro:tar fish$ unzip cat.zip
Archive:  cat.zip
replace cat.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
replace cat1.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: n
````
### rar
**linux环境需要下载RAR for linux，用法与zip类似**
```shell
#下载rar安装包
$ tar -xzpvf rarlinux.tar.gz
$ cd rar
$ make
```

##参考文档：
[linux压缩（解压缩）命令详解](http://blog.csdn.net/hbcui1984/article/details/1583796)  
[linux命令手册](http://linux.51yip.com/search/tar)


