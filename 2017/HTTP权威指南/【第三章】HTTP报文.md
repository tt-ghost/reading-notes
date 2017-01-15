
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



