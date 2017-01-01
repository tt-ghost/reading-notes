
> 慕课网学习教程

### 程序实体与关键字

- 名字首字母为大写的程序实体可以被任何代码包中的代码访问到。
- 而名字首字母为小写的程序实体则只能被同一个代码包中的代码所访问。 

### 关键字

|用途|关键字|
|----|-----|
|程序申明|import, package|
|程序实体声明和定义|chan, const, func, interface, map, struct, type, var|
|程序流程控制|go, select, break, case, continue, default, defer, else, faultthrough, for, goto, if, range, return, switch|

### 变量和常量

- var 用于变量申明，可以申明不赋值
- const 用于常量申明，申明必须赋值

```go
var num1 int = 1
// 或
var num2, num3 int = 2, 3
// 或
var ( // 多行赋值
  num4 int = 4
  num4 int = 5
)
```
