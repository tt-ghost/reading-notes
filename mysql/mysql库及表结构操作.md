# SQL 学习笔记

## 数据库结构操作

#### 创建数据库并设置字符集
- 设置为utf8

```sql
create database if not exists test_db default charset utf8 collate utf8_general_ci;
```

- 设置为gbk

```sql
create database yourdb DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
```

- 另一种修改字符集

进入到mysql安装目录，修改my.ini文件，将 `character_set_database = gbk` 修改为 `character_set_database = utf8`

#### 删除数据库

```sql
drop database test_db;
```

#### 查询数据库字符集

```sql
show variables like 'character_set_%';
```

#### 修改字符集编码

```sql
set character_set_database=utf8;
```

#### 选择数据库

```sql
use test_db;
```

