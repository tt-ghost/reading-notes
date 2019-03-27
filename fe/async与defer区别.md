# async 与 defer区别

![](./img/defer-async-script.png)

注：
1、在使用 `defer` 时，html解析之后会执行js，但是在执行js之前需要等待`CSSOM`构建完成，然后执行js，然后出发 `DOMContentLoaded` 事件

参考：
- [html5 syntax](https://www.w3.org/TR/html5/syntax.html#the-end)
