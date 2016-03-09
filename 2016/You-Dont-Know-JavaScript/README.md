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
