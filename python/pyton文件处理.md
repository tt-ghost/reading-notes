## python文件处理
### 一、linux系统基础
```linux
>> ls -l #查看当前文件夹文件
#显示如下
-rwxrw-r-x # 一个字母为文件类型，-为文件，d为文件夹，l为连接，后面每三个为一组，第一组为当前用户有读写执行权限，第二组为当前用户组对文件有读写权限，没有执行权限，第三组为其他用户对这个文件有读和执行权限，没有写权限
```

### 二、打开文件方式
- 1、open(name[,mode])
- 2、读取方式
```python
read([size]) #读取文件，不指定size默认读取全部，指定的话读取size字节大小
readline([size]) # 读取一行，size为字节数
readlines([size]) #读取完文件（其实只有缓存大小个字节），返回每一行说组成的列表
iter:使用迭代器读取文件，不好用内存的方式对文件进行遍历
```
- 3、写入文件
```python
write(str) # 写入指定的字符串
writelines(s) # 写多行到文件，s为字符串组成的列表
```
- 4、python文件打开方式
```python
mode    说明                           注意
‘r’          只读方式打开            文件必须存在
‘w’         只写方式打开            文件不存在创建文件，存在就清空
‘a’          最佳方式打开            文件不存在就创建
‘r+’/‘w+’ 读写方式打开            
‘a+’        追加和读写方式打开
‘rb’,’wb’,’ab’,’rb+’,’wb+’,’ab+’:以二进制方式打开
```
- 5、
  - 文件写入与写缓存
  - python写磁盘时机
    - a：主动调用close()或flush()方法，写缓存同步磁盘
    - b：写入数据大于或等于写缓存，写缓存同步磁盘
- 6、file.fileno #当前文件打开的数量
```python
list_f = []
for i in range(1025):
    list_f.append(open(’test.txt’,’w’))
    print ‘cur:%s ; fileno:$s’ %(i,list_f[i].fileno)
```
- 7、python文件指针
```python
#python文件指针定位方式
os.SEEK_SET #相对文件起始位置
os.SEEK_CUR #相对文件当前位置
os.SEEK_END #相对文件结尾位置
f.open(’test.txt’,’r+’)
import os
f.tess() # 读取当前指针位置
f.read(3) #012 ， 读取三个字节
f.tell() #3，当前指针在3的位置
f.seek(0,os.SEEK_SET) #指针到起始位置
f.tell() #0
f.seek(0,os.SEEK_END) #到文件结尾
f.seek(-5,SEEK_CUR) # 将指针从当前位置向前移动5个
# 用seek()设置指针超出去文件文件指针范围时，会报错
```

### 三、文件属性编码格式
- 1、
  - file.fileno()：文件描述符
  - file.mode：文件打开权限
  - file.encoding：文件编码格式
  - file.closed：文件是否关闭
- 2、文件标准
  - 文件标准输入：sys.stdin
  - 文件标准输出：sys.stdout
  - 文件标准错误：sys.stderr
```python
import sys
type(sys.stdin) #file，是文件类型
sys.stdin.mode # ‘r’ 只读
sys.stdin.read() #只有输入，没有输出
sys.stdin.fileno() # 0，打开标准输入
a = raw_input(“;”) #其实调用sys.stdin读入文件的，然后是输入模式
a # 输出刚才输入的内容
sys.stdout.write(’ssss’) #ssss
print ‘aaa’ #其实调用sys.stdout.write()方法
sys.stderr.mode # ‘w’
sys.stderr.write(‘aa’) #aa
sys.stderr.fileno() #2
```
- 3、文件命令行参数
  - sys模块提供sys.argv属性，通过该属性可以得到命令行参数；
  - sys.argv：字符串组成的列表
```python
#写个方法打印参数
vi arg.py
import sys
if __name__ == ‘__main__’: #判断是否是入口函数
    print len(sys.argv)
    for arg in sys.argv:
        print arg
python arg.py 0 1 2
#4 arg.py 0 1 2     //4个参数，第一个是文件名
```
- 4、文件编码格式
```python  
u’测试’ # 如果文件是unicode编码，会报错
f = open(’test.txt’,’r+')
a = unicode.encode(u’测试’,’utf-8’)
f.write(a) #
#如何创建一个utf-8或其他格式文件
#使用codecs模块提供的创建指定编码格式文件
#open(fname,mode,encoding,errors,buffering)
import codecs
f = codecs.open(’text.txt’,’w’,’utf-8’)
f.encoding # ‘utf-8'
f.write(u'测试’)
f.close()
```
- 5、使用os模块打开文件
  - a:
```python
#os.open(filename,flag[,mode]) # 打开文件
#flag:打开文件方式
#os.O_CREATE：创建文件
#os.O_RDONLY：只读方式打开
#os.O_WRONLY：只写方式打开
#os.O_RDWR：读写方式打开
```
  - b:使用os模块对文件进行操作
```python
#os.read(fd,buffersize) ：读取文件，fd为文件描述法
#os.write(fd,string) ：写入文件
#os.lseek(fd,pos,how)：文件指针操作
#os.close(fd):关闭文件
import os
fd = os.open(’test.txt’,os.O_CREATE | os.O_RDWR) #读写模式打开文件
n = os.write(fd,’test’)
l = os.lseek(fd,0,os_SEEK_SET)
str1 = os.read(fd,2)
str1 #te
os.close(fd)
#os模块方法介绍
os方法                             说明
access(path,mode)          判断该文件权限：F_OK存在，权限：R_OK,W_OK,X_OK
listdir(path)                      返回当前目录下所有文件组成的列表
remove(path)                   删除文件
rename(old,new)              修改文件或目录名
mkdir(path[,mode])          创建目录
makedirs(path[,mode])    创建多级目录
removedirs(path)             删除多级目录
rmdir(path)                      删除目录（必须为空）
#eg
os.access(’test.txt’,os.F_OK) # True，说明有当前文件
os.listdir(‘./‘) # [’test.txt']
#os.path模块方法介绍
os.path方法            说明
exists(path)            当前路径是否存在
isdir(s)                    是否是一个目录
isfile(path)              是否是一个文件
getsize(filename)    返回文件大小
dirname(p)             返回路径目录
baseanme(p)          返回路径的文件名
#eg
os.path.exists(’text.txt’) # True
```

### 四、python配置模块
```python  
#ini配置文件格式：
节：                  [session]
参数（键＝值）  name = value
#eg:
[port]
port1 = 8000
port2 = 9000
#eg
import ConfigParser
cfg = ConfigParser.ConfigParser()
cfg.read(’test.txt’) #[’test.txt’]
cfg.sections() # 返回session组成的列表
```
