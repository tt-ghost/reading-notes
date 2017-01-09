
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


