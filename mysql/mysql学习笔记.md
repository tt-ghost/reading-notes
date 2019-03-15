## MYSQL 笔记
### 一、查询
#### 1、where子句
```sql
select * from user where userId >10;
select * from user where userId between 10 and 20; -- 还可以not between
select * from user limit 10;
```

- 选择user中age在10与20之间且name不为xiaowang或xiaoli的所有记录
```sql
select * from user where (age between 10 and 20) and not name in (‘xiaowang’,’xiaoli’);
```

- nameType不在C和M之间的所有记录
```sql
select * from user where nameType not between ‘C’ and ‘M’;
```

- 两个时间点的记录
```sql
select * from user where date between #07/04/1900# and  #04/03/1901#;
```

- 注:
  - 1)、<> //为不等于
  - 2)、between v1 and v2 //再v1和v2之间的值
  - 3)、like模式匹配 %匹配0个或多个，_ 匹配一个字符，[a-c] 含有a或b或c的，[!ab]或[^ab]，不含有a且不含b
```sql
select * from user where name like ‘%xiao%’; -- 找到所有name中含有xiao的记录
select * from user where name not like ‘%xiao%’; -- 找到所有name中不含有xiao的记录
```

#### 2、order by
- 降序desc , 升序asc , 优先order by 后的第一个字段排序
```sql
select * from user order by userId,email desc;
```

#### 3、选择返回指定前多少条数据  
```sql
select top 15 userId from user;
```

#### 4、in在where子句中规定多值
- 筛选user表中name为xiaoming或xiaowang的记录
```sql
select * from user where name in (‘xiaoming’,’xiaowang’);
```

#### 5、连表查询
- inner join表中至少一个匹配才返回，left join 为即使右表没有匹配，也从左表返回所有记过
right join 与left join相反；full outer join 只要其中一个表中存在匹配，则返回行
- on为连接条件
```sql
select table1.name,table2.email from table1 inner join table2 on table1.userId = table2.userId;
```

#### 6、选择并插入到其他表
```sql
select * user.userId,sale.saleId into newTable from user left join sale on user.userId = sale.userId;
```


### 二、插入行 insert into
```sql
insert into user (name,mob,email) values (‘xiaoming’,18787877777,’xiaoming@163.com’);
insert into user values ();
```

### 三、更新数据 update table_name set
```sql
update user set email = ’xiaowangtest@126.com’ ,mob=‘12121211212' where email=’xiaowangtest@163.com’;
```
- 注:务必带上where子句，不然会批量更新所有数据；

### 四、删除记录
```sql
delete from table_name where name=‘xiaoming’;
-- 删除表中所有记录
delete from table_name;
-- 或者
delete * from table_name;
```
