
# HTTP报文

### 报文的组成部分

> 由三部分组成，对报文进行描述的起始行(start line)、包含属性的首部(header)块，以及可选的、包含数据的主体(body)部分

- 起始行和首部是由行分隔的ASCII文本。
- 每行都以由两个字符组成的行终止符作为结束，一个是回车符(ASCII为13)，另一个是换行符(ASCII为10)。行终止序列可以写做`CRLF`
- 稳健的应用程序应该在接受单个结束符时也能作为行终止。

#### 报文的语法

- 请求报文格式：

```xml
<method> <request-URL> <version>
<headers>

<entity-body>

// 例如
GET /user HTTP/1.1
.....
```
- 相应报文格式：

```xml
<version> <status> <reason-phrase>
<headers>

<entity-body>

// 例如
HTTP/1.1 200 OK
...
```

**注意：**

> 一组HTTP首部(headers)总是应该以一个空行(单个CRLF)结束，即使没有首部和实体的主体部分也应该如此。因历史原因很多客户端和服务器在没有实体主体部分时，错误的省略了CRLF，为与这些流行但不符合规则的实现互通，服务端、客户端应该接受那些没有最后CRLF的报文

#### 起始行

- 请求行是请求报文的起始行

- 相应行是响应报文的起始行

- 方法：

|方法|描述|是否包含主体|
|---|----|----------|
|HEAD|获取文档首部|否|
|POST|发送需要处理的数据(编辑)|是|
|POST|将主体部分存在服务器(创建)|是|
|TRACE|对可能经过代理服务器传送到服务器上去的报文进行追踪|否|
|OPTIONS|决定在服务器上可以执行哪些方法|否|

- 版本号：

比较HTTP版本时，每个数字都要进行单独比较，比如`HTTP/2.22`比`HTTP/2.3`版本高 

#### 首部

- 首部分为 **通用首部**、**请求首部**、**响应首部**、**实体首部**、**扩展首部**
- 实体首部描述主体的长度和内容，或者资源自身
- 扩展首部，规范中没有定义的新首部
- 首部延续行，将首部行分为多行可以提高可读性，多出来每行前面至少要有一个空格或制表符

```js
HTTP/1.0 200 OK
Content-Type: image/gif
Content-Length: 3452
Server: Test Server
  Version 1.0
```

### 方法

#### 安全方法有`GET`和`HEAD`

#### HEAD允许客户端在未获取实际资源情况下，对资源首部进行检查，用途：

- 在不获取资源的情况下了解资源的情况（如判断其类型）
- 通过查看相应状态码，看某个对象是否存在
- 通过查看首部，测试资源是否被修改

```js
curl -i -X HEAD 'http://www.baidu.com'
// 响应
Warning: Setting custom HTTP method to HEAD with -X/--request may not work the
Warning: way you want. Consider using -I/--head instead.
HTTP/1.1 200 OK
Server: bfe/1.0.8.18
Date: Thu, 19 Jan 2017 03:17:12 GMT
Content-Type: text/html
Content-Length: 277
Last-Modified: Mon, 13 Jun 2016 02:50:04 GMT
Connection: Keep-Alive
ETag: "575e1f5c-115"
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Pragma: no-cache
Accept-Ranges: bytes

curl: (18) transfer closed with 277 bytes remaining to read
```

查看资源是否存在

```js
curl -i -X HEAD 'https://ss0.bdstatic.com/5aV1bjqh_Q23odCf/static/superman/img/logo/logo_white_fe6da1ec.png'
// 响应
Warning: Setting custom HTTP method to HEAD with -X/--request may not work the
Warning: way you want. Consider using -I/--head instead.
HTTP/1.1 200 OK
Server: bfe/1.0.8.13-sslpool-patch
Date: Thu, 19 Jan 2017 03:18:36 GMT
Content-Type: image/png
Content-Length: 6958
Connection: keep-alive
ETag: "5874d951-1b2e"
Last-Modified: Tue, 10 Jan 2017 12:53:37 GMT
Expires: Tue, 14 Feb 2017 00:08:31 GMT
Age: 356841
Cache-Control: max-age=2592000
Accept-Ranges: bytes
Ohc-Response-Time: 1 0 0 0 0 0
```

#### TRACE

> 客户端发请求时，这个请求可能要穿越防火墙、代理、网关或其他应用，每个环节都可能修改HTTP请求。TRACE方法允许客户端在最终将请求发到服务端时，看它变成什么样

- TRACE请求会在目的服务器端发起一个“环回”诊断
- 行程最后一站的服务器会弹回一条TRACE响应，并在响应主体中携带它收到的原始请求报文。
- TRACE主要用来诊断，用于验证请求是否如愿穿过了请求/响应链。
- 缺点是TRACE假定中间应用程序对各种不同请求处理是相同的。实际上很多HTTP应用汇根据方法做不同的事情。通常中间应用程序自行决定对TRACE请求的处理方式。
- TRACE不能带主体部分
- TRACE响应实体主体部分包含响应服务器收到请求的精确副本

#### OPTIONS 

- OPTIONS方法请求服务器告知其支持的方法。询问支持哪些方法或特殊资源支持哪些方法
- 跨站请求时浏览器会先发送一个OPTIONS请求

#### 扩展方法

- 常见有`LOCK`、`MKCOL`、`COPY`、`MOVE`等
- 如果带有未知方法的请求时，通常以501 Not Implemented(无法实现)状态返回
- 请求的发送接收一般按惯例：对说发送的内容要求严一些，对接收的内容宽容一些

### 状态码

#### 100~199 信息状态码

- `100 Continue`，主要用于优化，客户端只有避免向服务器发送一个服务器无法处理或使用的大实体时，才应该使用 `100 Continue`，即带 `Expect: 100 Continue` 的头部，并且在等待服务器响应一定时间后，客户端应该直接将实体发送出去
- 服务端收到 `Expect: 100 Continue` 的头，用`100 Continue`或错误码响应
- 代理收到`100 Continue`期望的请求时，应将`Expect: 100 Continue`向下转发。如果知道下一个服务器只能与HTTP/1.1之前的版本兼容，就应该以`417 Expectation Failed`错误进行响应
 
#### 200~299 成功状态码

|状态码|原因短语|
|-----|------|
|200|OK|
|201|Created|
|202|Accepted|
|203|Non-Authoritative Infomation|
|204|No Content|
|205|Reset Content|
|206|Partial Content|

#### 300~399 重定向状态码

|状态码|原因短语|
|-----|------|
|300|Multiple Choices|
|301|Moved Permanently|
|302|Found|
|303|See Other|
|304|Not Modified|
|305|User Proxy|
|306|(未使用)|
|307|Temporary Redirect|

#### 400~499 客户端错误状态码

|状态码|原因短语|
|-----|------|
|400|Bad Request|
|401|Unauthorized|
|402|Payment Required|
|403|Forbidden|
|404|Not Found|
|405|Method Not Allowed|
|406|Not Acceptable|

