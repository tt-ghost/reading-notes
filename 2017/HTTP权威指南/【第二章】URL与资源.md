
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