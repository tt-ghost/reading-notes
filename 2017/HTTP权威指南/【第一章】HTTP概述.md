
# HTTP概述

### 一、媒体类型

#### 1、HTTP会为每种通过WEB传输的对象打上MIME类型(Multipurpose Internet Mail Extension，多用途因特网邮件扩展)

- MIME类型是一种文本标记，表示一种主要对象类型和一个特定的子类型
- 常见类型如：
  - HTML为 `text/html`
  - 普通ASCII为 `text/plain`
  - JPEG图片为 `image/jpeg`
  - GIF为 `image/gif`
  - Apple 的QuickTime为 `video/quicktime`
  - PowerPoint为 `application/vnd.ms-powerpoint`类型 

#### 2、URI,服务器资源名被称为*统一资源标识符*(Uniform Resource Identifier,URI)

#### 3、URL,*统一资源定位符*，几乎所有URI都是URL

#### 4、URN,*统一资源符*


### 二、连接

#### 1、TCP/IP

HTTP是应用层协议，联网细节有通用、可靠的因特网传输协议`TCP/IP`(Transmission Control Protocol)控制

```
应用层         HTTP

传输层         TCP

网络层         IP

数据链路层  网络特有的链路接口

物理层       物理网络硬件
```

#### 2、使用Telnet实例

```shell
// 命令行
$ telnet www.baidu.com 80
// 输出结果
Trying 61.135.169.121
Connected to www.a.shifen.com.
Escape character is '^]'.
// 交互性输入
GET / HTTP/1.1
Host: www.baidu.com
// 输出
HTTP/1.1 200 OK
Date: Tue, 10 Jan 2017 13:06:12 GMT
Content-Type: text/html
...


<!DOCTYPE html><!--STATUS OK-->
...
</body></html>Connection closed by foreign host.

```

**说明：**

1. Telnet会查找主机名并打开一条连接，连接到在`www.baidu.com`的端口为80上监听的Web服务器，之后三行为Telnet输出，连接结果
2. 然后输入`GET / HTTP/1.1` 发送一个提供了源端主机名的`Host`首部，后面跟一空行
3. Telnet可以很好的模拟HTTP客户端。更灵活工具可以看看`nc`(netcat)，方便的操纵`UDC`和`TCP`的流量。

### 三、协议版本

目前在使用的版本有`HTTP/0.9`、`HTTP/1.0`、`HTTP/1.0+`、`HTTP/1.1`(当前版本)，还有`HTTP-NG`(又名`HTTP/2.0`)

### 四、Web的结构组件

代理、缓存、网关、隧道、Agent代理
