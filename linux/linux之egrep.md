## egrep

### 用法：
用法及参数与 `grep -E` 差不多，具体可参考 [linux之grep](./linux之grep.md)，不同点在于解读字符串的方法：
`egrep` 是用 **extended regular expression** 语法来解读，而`grep`用 **basic regular expression** 语法解读。**extended regular expression** 比 **basic regular expression**的表达更规范

语法：`egrep(选项)(查找模式)(文件1, 文件2...)`

