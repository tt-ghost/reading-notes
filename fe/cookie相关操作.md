# cookie相关操作

> 前端通过js操作浏览器cookie

## 事例

```
var expire = new Date(new Date().valueOf() + 1000*60*60*24*2).toGMTString();
document.cookie=`name=test;domain=aa.com;path=/path-a;expire=${expire};secure;`;
// 或许
document.cookie=`name=test;domain=aa.com;path=/path-a;max-age=100;secure;`;
```

注意：
- 设置和删除是一个操作，只要在设置的时候修改响应字段就可以删除
- 页面可以读取跟自己相同或者上一级path的cookie， 无法读取自己路径，或者不同路径的cookie
- cookie中的key 和 value如果含有特殊字符，使用`escape/encodeURIComponent`编码，`unescape/decodeURIComponent`解码
- 如果通过`ajax`通过设置http header的Cookie字段将报错，会忽略 `setRequestHeader`

```js
ar client = new XMLHttpRequest();
client.open('GET', '/');
client.setRequestHeader('Test', 'one'); // 正确
client.setRequestHeader('Cookie', 'a=c'); // 失败
// Refused to set unsafe header "Cookie"
```

## 属性介绍

- `name=test`是cookie的KV，必填
- `domain`为设置的域名，同`path`类似，只可以设置当前域或根域，但不能设置`.com`域，如当前域为`a.b.com`，可以设置在 `a.b.com`或 `b.com`，但不能设置`.com`域下。另外value前带不带点有区别：
    + 带点：任何子域都可以访问，包括父域
    + 不带点：只有完全一样的域名才能访问，子域不能访问，IE特殊可以
- `path`是cookie的路径，非必填项，默认为当前路径;如果当前在 `/path-b`路径下，设置`/path-a`路径的cookie会不生效；但是可以设置`/`路径的cookie
- `expire`或者 `expires`为过期时间，非必填，默认在回话结束cookie会失效，必须为`GMT`或`UTC`时间（二者可以认为一致）,`expire`为`http/1.0` 协议的选项，`http/1.1`有`max-age`代替，同时使用时`max-age`会优先使用
- `max-age`，`http/1.1`协议替换之前的`expire`，非必填，默认值`-1`(即有效期为`session`)。单位为秒，取值有三种：
    + 负数：有效期为 `session`
    + 0：删除 `cookie`
    + 正数：有效期为创建时刻 + `max-age`
- `secure` 非必填，只有 `https` 站点才能设置 `secure` 的cookie，如果是`http`站点，执行的话将会删除对应cookie；
- `HttpOnly` 前端无法直接设置，也无法读取，必须服务端设置，浏览器在请求时会自动带上`HttpOnly`的cookie

## 参考文档

- [聊一聊 cookie](https://segmentfault.com/a/1190000004556040)
