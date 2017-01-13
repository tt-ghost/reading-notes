
# URL与资源

### URL语法

- 大多数URL都由以下9部分构成

```
<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<flag>
```

#### 方案(scheme):必须以字母符号开头，由第一个`:`符号与URL其他部分分隔，大小写无关

#### 参数，由`;`分隔

```
http://a.com/ha;sale=false/index.html;gra=true
```
这里两个路径段，`ha`和`index.html`.`ha`路径段有参数`sale`，值为`false`;`index.html`段有参数`gra`，值为`true`

#### 查询字符串，由`?`分隔

### 字符

#### 编码机制

- 特殊字符转义，跟`%`，用ASCII的16进制表示，比如空格十进制为32，十进制为`0x20`，URL中就是`%20`

#### 字符限制

### 方案的世界(scheme)

常见方案格式

|方案|描述|
|----|---|
|http|没有用户名和密码，其他与通用url格式相符，默认端口80|
|https|与http对应，区别在于使用了`SSL`加密，默认端口443|
|mailto|语法基础参见[RFC 822](http://www.ietf.org/rfc/rfc0822.txt)|
|ftp|格式：`ftp://<user>:<password>@<host>:<port>/<path>;<params>`|
|rtsp,rtspu|RTSP实时流传输协议(Real Time Streaming Protocol)解析音视频。`rtspu`的`u`表示使用`UDP`协议来获取资源。基本格式：`rtsp://<user>:<password>@<host>:<port>/<path>`，`rtspu://<user>:<password>@<host>:<port>/<path>`|
|file|指定主机（本地磁盘、网络文件系统、其他文件共享系统）上直接访问文件。如`file://OFFICE-FS/policies/a.doc`|
|news|用来访问一些特定的文章或新闻组，new URL自身包含的信息不足以对资源进行定位。`@`用来区分指定新闻组的news URL和指向特定新闻文章的news URL。基本格式：`news:<newsgroup>`和`news:<news-article-id>`，如:`news:rec.arts.startrek`|




