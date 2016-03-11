# 你不知道的JavaScript

## 第一部分 作用于和闭包

### 第一章 作用域是什么

#### 编译原理
通常讲JavaScript归类为“动态”、“解释执行”语言，事实上它是一门编译语言。源代码在执行前会经历三个步骤：
+ 分词/词法分析(Tokenizing/Lexing)
  + 如<code>var a = 2;</code> 会分解成以下词法单元<code>var</code> <code>a</code> <code>=</code> <code>2</code> <code>;</code>；
+ 解析/语法分析(Parsing)
  + 将上一步的词法单元流（数组）转成嵌套的 ”抽象语法树“(Abstrac Syntax Tree. AST);
  + <code>VariableDeclaration(var)</code> -> 
    + <code>Identifier(a)</code> -> 
      + <code>AssignmentExpression(=)</code> -> 
        + <code>NumericLiteral(4)</code>
+代码生成
将AST转为可执行代码的过程。即将<code>var a = 2;</code>的AST转为机器指令，创建变量，分配内存等。
  + JavaScript 引擎要比三步骤复杂的多
  + JavaScript 的编译不是在构建之前，而是在执行前编译。

#### 小结

+ **LHS**、**LHS** 查询：如果查找的对象是对变量进行赋值，则进行LHS查询；如果目的是获取变量的值，则会使用RHS查询，赋值操作符会导致LHS查询。<code>=</code>操作符或调用函数时传入参数的参数的操作都会导致关联作用域的赋值操作。
+ JavaScript引擎在代码执行前进行编译。像<code>var a = 2</code>这样的声明会被分解一下两步：
  + <code>var a</code>在其作用域中申明新变量，这会在代码执行前发生。
  + <code>a = 2</code>会查询（LHS查询）变量a并对其赋值。、
+ LHS 和 RHS 查询都会在当前执行作用域中开始。如果当前作用域中没有找到需要的标识符，就到上级作用域中查找，直到顶层作用域，没有找到就会停止。
+ 不成功的RHS查询会导致<code>ReferenceError</code>异常，不成功的LHS会导致自动隐式的创建一个全局变量（非严格模式下），该变量使用LHS引用的目标作为标识符。或者抛出<code>ReferenceError</code>异常（严格模式下）

### 第二章 词法作用域

#### 词法阶段
+ 作用域共有两种主要的工作模型：
  + 词法作用域，大部分语言都是这种
  + 动态作用域，如Bash、Perl等
+ 词法作用域是由你写代码时将变量和块作用域写在哪里决定的。
+ 作用域查找会在找到第一个匹配的标识符时停止。

#### 欺骗词法
+ 欺骗词法作用域会导致性能下降
+ eval




