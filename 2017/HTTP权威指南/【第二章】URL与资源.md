
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
|http|没有用户名和密码，其他与通用url格式相符，端口默认80|

