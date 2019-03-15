## shell学习笔记

```shell
#!/bin/bash -xv #调试bash脚本需要带上-xv参数,bash脚本的第一个字符为#
#!/bin/sh #表示用sh脚本
#!/usr/bin/python #表示用python脚本
:<<'EOF' #块注释
cat $1
echo $1 #获取文件第一个参数
echo $0 #获取脚本名称
echo $? #检查之前的命令是否运行成功
tail -2 #获取文件最后两行
head -2 #获取文件前两行
awk '{print $3}' $1 #获取一个文件第一行的第三个元素
awk '{if ($3 == "at") print $2}' $1 #如果文件每行第三个元素是at，则打印第二个元素
echo $* #打印出脚本的所有参数,字符串形式
echo $@ #同上,以$IFS为分隔符
echo $$ #输出当前shell的PID
echo $# #输出传递给脚本的参数数目
```

### 创建函数
```shell
function example(){
echo 'Hello World!'
}
```

### 连接字符串
```shell
v1="Hello" #注意等号两边不能有空格
v2="Bash"
v3=${v1}${v2}
echo $v3
```

### 加法
```shell
v1=1 #注意等号两边不能有空格
v2=2
v3=$v1+$v2
let v4=$v1+$v2 #3
echo $(($v1+$v2)) #3
echo $[$v1+$v2] #3
expr $v1 + $v2 #3
echo $v1+$v2  | bc #3
echo $v3 #1+2
echo $v4 #3
```

### 检查文件是否存在
```shell
if [ -f ./out.log ]
then
echo "File exists"
fi
```

### for循环
```shell
ls="10 20 32 34"
for i in $(ls);do
echo item:$i
done
```

### while循环
```shell
COUNTER=0
while [ $COUNTER -lt 10 ]; do
echo The counter is $COUNTER
let COUNTER=COUNTER+1
done
```


### until循环
```shell
COU=12
until [ $COU -lt 5 ]; do
echo The COU is $COU
let COU-=1
done
```

### 命令export表示在子shell中使用
### 后台运行脚本是在脚本后面添加&
### &&为当前脚本成功执行后才执行后面的脚本
### 单引号中命令不能运行，双引号相反

### 用echo获取字符串的一部分
```shell
varstr='test hello money'
x=3
y=7
echo ${varstr:x:y} #x为其实位置,y为长度
```


### 用echo借去home_dir,vars="User:123:321:/home/dir"
```shell
vars="User:123:321:/home/dir"
echo ${vars#*:*:*:} #截取
echo ${vars##*:} #效果同上
echo ${vars%:*:*:*} #获取User
echo ${vars%%:*} #效果同上
```

### 获取变量长度
```shell
varss='testvarhelloobash'
echo ${#varss}
echo ${varss: -3} #打印变量最后三个字符
echo ${varss:-3} #如果之前没有给varss赋值，则输出3，否则输出该变量
echo ${varss//test/ghost} #用echo替换字符的一部分,第一个字符串为需要替换的字符，后一个为替换后的字符
```

### 列出第二个字母是a或b的文件
```shell
ls -d ?[au]*
```

### 去除字符串里的所有空格
```shell
stra='te stsa a asdfe host'
echo $stra|tr -d " "
```

### 写出0-100中3的倍数
```shell
for i in {0..100..3}
do
echo $i
done
#或者
for (( i=0; i<=100; i=i+3))
do
echo "hello $i"
done
EOF
```

- [ $a == $b ]用于字符串比较，[ $a -eq $b ]用于素质比较
- [$a -gt 12] 为变量a是否大于12
- [$a -le 12] 为变量a是否小于12

- [[ $string == abc* ]] 表示判断字符串是否已abc开头

- 列出以t或a开头的用户名
```shell
egrep "^t|^a" /etc/passwd|cut -d: -f1
```

- bash中#!表示后台最后执行命令的PID
- 而$?表示前台最近命令的结束状态

### 定义是数组
```shell
array=("HI","MY","NAME")
echo $array
echo ${!array[@]} #输出数组索引
unset array[1] #移除索引为1的元素
array[22]="ghost test" #添加id为22的元素
echo $array
```

- 通过read获取输入的值
