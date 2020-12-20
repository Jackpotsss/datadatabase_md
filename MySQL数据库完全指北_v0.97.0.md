# 目录

[TOC]



# 1. MySQLUGtdySjuV

​	Mysql是一个关系型数据库管理系统。属于Oracle旗下的开源产品，分为社区版和商业版。



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200217164623051.png" alt="image-20200217164623051" style="zoom:150%;" />



## 1.1 MySQL逻辑架构



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200218141709678.png" alt="image-20200218141709678" style="zoom:80%;" />



MySQL的逻辑架构分为三层：

- **连接层**
- **服务层**
- **存储引擎**



​	第一层是连接层，连接层和大多数基于网络的应用一样，用于连接处理、授权认证、安全校验等。

​	第二层是服务层，**缓存、语法解析器、优化器；函数、视图、存储过程、触发器**都在服务层实现。

​	第三层是存储引擎，Mysql支持多种可插拔式的存储引擎。存储引擎只负责数据的存储和提取，存储引擎之间没有相关性，只是简单的响应上层服务的请求。



MySQL查询过程

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200212174722495.png?raw=true" alt="image-20200212174722495" style="zoom: 80%;" />



## 1.2 MySQL时间线

**版本3.23（2001年）**

​	这个版本是MySQL的真正诞生版本。

**版本4.0（2003年）**

**版本5.0（2006年）**

**版本5.1（2008年）**

- 这是 Sun 收购 MySQL AB 以后发布的首个版本，研发时间长达五年。
- 同时 Oracle 收购的 InnoDB Oy发布了 InnoDB plugin。

**版本5.5（2010年）**

- Oracle 收购 Sun 以后发布的首个版本。
- InnoDB 成为默认的存储引擎。
- 该版本在性能和稳定性上有了很大的提升。

**版本5.6（2013年）**

**版本5.7（2015年）**

**版本8.0（2018年）**

MySQL大概3年发布一个大版本，产品支持的周期一般是8年。

## 1.3 版本

本文基于 mysql5.6 版本学习。

## 1.4 官网

MySQL官网：		www.mysql.com

MySQL官方文档：dev.mysql.com/doc

# 2. SQL 语言UGtdySjuV

SQL（Structure Query Language）是结构化查询语言的缩写。SQL 可分为如下几类：



**DDL（Data Definition Languages）数据定义语句：**

​	数据定义语言，这些语句定义了不同的数据库、表、列、索引、视图等数据库对象的定义。主要命令包括 create、drop、alter。

**DML（Data Manipulation Language）数据操作语句：**

​	数据操纵语句，用于添加、删除、更新和查询数据库记录，并检查数据完整性，主要命令包括 insert、delete、udpate 和select 。

**DCL（Data Control Language）数据控制语句：**

​	数据控制语句，用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库、表、字段、用户的访问权限和安全级别。主要命令包括 grant、revoke 。



## 2.1 DML 数据操作语言

​	通过数据操作语言DML（Data Manipulation Language），可以实现对数据库中数据的增删改查操作，DML 涉及的语句有 SELECT、INSERT、UPDATE、DELETE 。

### SELECT

**说明** `select` 是SQL中的查询语句。

**语法** 

```mysql
SELECT [DISTINCT]
	<列表达式>,<列表达式>...
FROM
	<表名/视图名>,<表名/视图名>...
[WHERE]
	<条件表达式>
[GROUP BY]
	<列名>
[HAVING] <条件表达式>
[ORDER BY] <列名> [DESC | ASC]
```

例子:

```mysql
# 查询指定列
select column_name from table_name;
# 查询所有列
select * from table_name;
```

### DISTINCT

**说明**：去重语句 `distinct` 。

**语法** 

```sql
SELECT DISTINCT column_name FROM table_name;
```



### JOIN ON

**说明** ： `JOIN ON` 用于连表查询。

连接方式分为四种:

​	内连接		(inner) join	：只返回符合关联条件的记录；

​	左外连接	left join		 ：返回与左表关联的全部记录；

​	右外连接	right join	  ：返回与右表关联的全部记录；

​	全外连接	full join		：MySQL不支持全外连接；



**语法**

```mysql
select * from t1 join t2 on t1.id = t2.id;			# 内连接
select * from t1 left join t2 on t1.id = t2.id;		# 左外连接
```

**建议** 

1. 连表数量不要太多，建议不要超过3个。
2. 连接条件的列考虑创建索引。

### WHERE

**说明**： `where`  用于条件过滤。

**语法** 

```sql
select t.id,t.name,t.age
from t
where t.age > 18;
```

**建议**

​	where中匹配的字段值要符合该字段的数据类型，避免Mysql进行隐式类型转换，这样会放弃索引。

### GROUP BY

GROUP BY 语句根据对**一个或多个**字段对结果集进行**分组**。

```mysql
# 在学生表中查询不同地区有多少人
select s.hometown,count(*) from student as s group by s.hometown;
# 在学生表中查询不同地区,不同年龄的有多少人
select s.hometown,s.age,count(*) from student as s group by s.hometown,s.age;
```



### HAVING

**说明**

​	`HAVING`  ：分组条件过滤。

​	`HAVING` 语句通常与 `GROUP BY` 语句联合使用，用来过滤由GROUP BY 语句返回的组记录集。

**用法**

```mysql
# 对查询出来的结果集进行过滤,下面这种写法是没问题的,不过这种情况可以使用where代替
select 
s.name,s.hometown
from 
student as s 
having s.hometown = "德州";
```

注意下面这几种写法是错误示范：

```mysql
# where后面不能跟聚合函数;
select 
s.hometown,count(*) 
from student as s 
group by s.hometown
where count(*) > 100;

# 不能对结果集中没有的字段进行having 过滤,此时只能使用where;
select 
s.name,s.hometown
from student as s 
having s.id = 2;

# group by 后面不能接where条件,必须放在group by 之前;
select 
s.hometown,count(*) 
from student as s 
group by s.hometown
where s.hometown = "德州";
```

注意:

1. having 的条件字段在select 子句中必须存在。（在查询出来的结果集中必须要有）
2. having 后面可以使用聚合函数。（where后面不可以使用聚合函数）



**having 和  where 的区别:**

​	where 对查询条件进行约束，即在返回结果集之前起作用；而且**where 后面不能使用聚合函数**；

having 对查询返回的结果集进行过滤，所以**having 的条件字段在查询出来的结果集中必须有！having 后面可以使用聚合函数。**

​	所谓聚合函数，就是对一组值进行计算后得到一个值，如 sum()，count()，avg()，max()；



在SQL中的执行顺序：

1. where
2. group by
3. having

having 和  where 组合使用：

```mysql
# 在学生表中查询年龄大于18并且人数超过100的家乡和数量
select 
s.hometown,count(*) 
from 
student as s 
where s.age > 18
group by s.hometown
having count(*) > 100;
```



### UNION (ALL)

**说明**

​	`UNION` 运算符用于**合并**两个或多个SELECT语句的**结果集**。

​	`UNION ALL` 不去重并集；`UNION` 去重并集；

**语法**

```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

**要求**

- UNION中的每个SELECT语句必须具有相同的列数
- 每个SELECT语句中的列也必须以相同的顺序排列
- 这些列也必须具有相似的数据类型



**建议**

​	尽量使用 `union all` ，减少使用 `union`。因为 union 去重，会做一次排序操作，以便删除重复项。union all 不去重，不做额外处理， `union all`的速度更快一些。

​	如果没有去重需要，优先使用 union all 。



### 运算符

MySQL 主要有以下四种运算符:

- 算数运算符
- 比较运算符
- 逻辑运算符
- 位运算符



**算数运算符**

| 运算符 | 描述 |
| ------ | ---- |
| +      | 加   |
| -      | 减   |
| *      | 乘   |
| /      | 除   |
| %      | 取余 |



**比较运算符**

| 运算符        | 描述                                   |
| ------------- | -------------------------------------- |
| =             | 等于                                   |
| <> 或 !=      | 不等于                                 |
| >             | 大于                                   |
| /             | 小于                                   |
| >=            | 大于等于                               |
| <=            | 小于等于                               |
| BETWEEN...AND | 在两值之间                             |
| IN            | 在集合中                               |
| NOT IN        | 不在集合中                             |
| <=>           | 严格比较两值是否相等 (可以比较null 值) |
| LIKE          | 模糊匹配                               |
| IS NULL       | 为空                                   |
| IS NOT NULL   | 不为空                                 |

注:

​	比较运算符常用于where子句中作为判断条件；

​	BETWEEN 1 AND 6 相当于 column  >= 1 AND column  <= 6；

​	column IN（1，2，3）相当于 column  =1 OR column =2 OR  column =3，IN 中的元素个数不应该过多；

​	尽量避免使用 否定条件：`<> 或 !=、NOT IN、IS NOT NULL`，否定条件会使索引失效，从而进行全表扫描，降低效率。



**逻辑运算符**

逻辑运算符用来判断表达式的真假，true 返回1，false 返回0；

| 运算符   | 描述     |
| -------- | -------- |
| AND      | 逻辑与   |
| OR       | 逻辑或   |
| NOT 或 ! | 逻辑非   |
| XOR      | 逻辑异或 |

逻辑与：两者都为true才返回true，其他情况返回false；

逻辑或：两者有一个为true，就返回true；两者都为false才返回false；

逻辑非：逻辑取反，常用于 NOT IN （不在集合中）和 IS NOT NULL（不是NULL）；

逻辑异或：两者只要不相等就返回true；两者相等返回false；



**位运算符**

位运算其实就是逻辑运算，只不过位运算的对象不是表达式，而是二级制的位（0或1）。

| 运算符 | 描述 |
| ------ | ---- |
| &      | 位与 |
| \|     | 位或 |
| ^      | 异或 |
| !      | 取反 |
| <<     | 左移 |
| \>>    | 右移 |



**BETWEEN...AND...**

**说明**

​	`between and` 用于范围条件查询。

**语法**

```mysql
#  范围查询
select * from students s where s.id between 1 and 10;	
#  等价于
select * from students s where s.id >= 1 and s.id <= 10;	
```



**IN | NOT IN 运算符**

**说明**

​	`in` 在某些固定值中查找，用于匹配多个值。

**语法**

```sql
select * from students s where s.name in ('Jack','Brian');
select * from students s where s.name not in ('Jack','Brian');
```

**建议**

​	IN 包含的值不应该过多，影响效率。



**LIKE 运算符**

**说明**

​	`LIKE` 运算符用于模糊匹配查询。

**语法**

```sql
SELECT id,name FROM table1 as t where name like '%jack'  -- 左模糊匹配
SELECT id,name FROM table1 as t where name like '%jack%'  -- 全模糊匹配
SELECT id,name FROM table1 as t where name like 'jack%'  -- 右模糊匹配
```

**建议**

​	不要使用左模糊和全模糊匹配查询，这样会使相关索引失效（不符合最左前缀匹配原则），尽量使用右模糊匹配。



### NULL 值处理 

**说明**

​	在WHERE 子句判断中遇到 NULL 值，处理结果可能和预期不一样，MySQL提供了三个运算符来判断NULL 值：

- `IS NULL`  		：当列值是 NULL，返回 true。
- `IS NOT NULL`  ：当列值不是 NULL，返回 true。
- <=>  ：当比较的的两个值相等或者都为 NULL 时返回 true。（区别于 = 运算符）

**语法**

```sql
select * from students s where s.phone is null;
select * from students s where s.phone is not null;
select * from students s where s.phone <=> null;
```

**注意**

​	MySQL中关于NULL 值的比较是比较特殊的，不能使用 = NULL 或 !=NULL 来查找NULL 值；

​	MySQL中NULL 值和任何值比较，都返回NULL ，即NULL = NULL 也是返回NULL；

​	但是NULL <=> NULL 返回true。



### ORDER BY

ORDER BY 子句用于对查询结果集进行排序。

- 可以设定多个字段进行排序；
- 默认为升序排列，可以使用 `ASC` 或 `DESC` 关键字设置结果集按升序还是降序进行排列；

```mysql
# 按成绩从高到低降序排名
SELECT * FROM student as s order by s.score desc;
```

### LIMIT

**说明**

​	`limit` 用于限制结果集的返回记录条数。第一个参数指定偏移量,第二个参数指定返回记录行的限制条数。

**语法**

```mysql
SELECT * FROM table1 limit 1000			# 查询前1000条数据
SELECT * FROM table1 limit 10000,10		# 查询第10001-10010条数据
```

**建议**

​	建议使用 limit N，减少使用limit M，N。特别是大表或偏移量M较大的时候。

​	MySQL 不支持 top 语句，使用limit 限制返回记录条数。

### SQL语句执行顺序

​	SQL语句的执行顺序和语法顺序是不一样的，这点需要额外注意。熟知SQL语句的执行顺序可以帮助你更好的理解和优化SQL，因此是非常有必要的。

```sql
from		<1>
on			<2>
join		<3>
where		<4>
group by	<5>
having		<6>
select		<7>
distinct	<8>
order by	<9>
limit		<10>
```



### 子查询 Subquery

子查询就是嵌套在主查询中的查询。常出现在 FROM、JOIN、WHERE中。

```mysql
select 
*
from bms_md_goods as g
where
g.recid in
(
select 
g.recid
from bms_md_goods as g
);
```

​	根据子查询和外部查询是否有关联，可以分为关联子查询和独立子查询。独立子查询可以单独运行，而关联子查询的一部分与外部查询关联。

```mysql
# 独立子查询
select *
from a join
(select b.id,sum(b.num) from b join c on b.cid = c.id group by c.id) as d
on a.bid = d.id;
# 关联子查询
select *
from a join
(select b.id,sum(b.num) from b where b.aid = a.id group by a.id) as d
on a.bid = d.id;
```



### 创建临时表

语法：

```mysql
# 创建临时表
CREATE TEMPORARY TABLE temp_table(
	select * from student
);
# 查询临时表
select * from temp_table;
```



- 临时表只在本次会话范围内有效，会话结束后，临时表会自动删除；
- 临时表在一次会话中名称不可以重复，但可以和物理表的表名重复；
- 临时表和物理表的表名重复情况下，临时表的优先级高于物理表；



当然也可以在本次会话结束之前手动删除临时表:

```sql
DROP TEMPORARY TABLE temp_table;
```

:warning: 特别注意:

​	使用 `DROP TABLE` 命令也可以优先删除临时表，但一定不要这样使用！因为临时表和物理表的名称可以重复，在重复的情况下执行两遍 `DROP TABLE` ，就把物理表也删除了，此操作很危险，尤其是在生产环境中；

​	临时表的删除不是必须的，如果非要删除，务必使用 `DROP TEMPORARY TABLE` 删除临时表，因为即便在与物理表重名的情况下重复执行该命令，也不会影响到物理表。



小结:

​	创建临时表可以在复杂的业务场景下，一定程度上提高SQL的可读性。它的用法和 `WITH AS` 很相似（MySQL不支持），可以互相替换。

​	MySQL不支持 with as 语句， 可用创建临时表代替 `CREATE TEMPORARY TABLE` ；

---

### 使用变量

在MySQL中，@ 和 @@ 符号用来操作变量。

- @ 符号用来操作用户自定义变量；
- @@ 符号用来操作系统变量（global 和 session）；



 **定义和使用自定义变量：**

```mysql
# 设置用户变量
set @date_from = STR_TO_DATE('2020-01-01 00:00:00','%Y-%m-%d %T');
set @date_to = STR_TO_DATE('2021-01-01 00:00:00','%Y-%m-%d %T');
# 使用变量
select * from student as s
where s.birthday BETWEEN @date_from AND @date_to ;
```

注：定义和使用用户自定义变量时，@符号都不可以省略；

**查询和设置系统变量：**

```mysql
# 查询系统变量
SELECT @@global.autocommit;		# 查看（全局）系统变量
SELECT @@session.autocommit;	# 查看（会话）系统变量
SELECT @@autocommit;			# @@ 如果不显式指定作用范围,则优先默认为会话范围,如果该变量没有会话,则默认是全局变量;
# 设置系统变量
SET GLOBAL autocommit = 0;		# 设置（全局）范围系统变量的值
SET SESSION autocommit = 1;		# 设置（会话）范围系统变量的值
SET autocommit = 1;				# 设置（会话）范围系统变量的值
```

注：

​	有的系统变量有全局和会话两个作用范围，有的则只有全局范围，没有会话范围，有的则只有会话，没有全局；

​	全局系统变量保存在 `information_schema.GLOBAL_VARIABLES` 表中;

​	会话系统变量保存在 `information_schema.SESSION_VARIABLES` 表中;



### insert into

数据插入语句：

```mysql
#  增加指定字段的值
insert into  student (id,`name`,age,version,hometown) values (19,'jack',15,"222","德州");
#  增加全部字段的值,可以省略 values 前面的字段名列表; 
insert into  student values (19,'jack',15,"222","德州");
#  批量增加,效率更高;
insert into  student values (19,'jack',15,"222","德州"),(20,'Tom',25,"223","北京");
```

注:

​	一次性增加很多数据时，强烈建议使用批量增加，可以大幅度提高数据插入效率，数据越多，效率提升越明显。



### insert into select 

**说明**

​	`insert into select ` 语句用于从一个表复制数据，插入到另一个现有表中，目标表中任何已存在的行都不会受影响。

**语法**

```mysql
# 复制全部数据到已有表中;
insert into table2 select * from table1;
# 只复制指定列的数据到已有表中;
insert into table2 (column_name(s)) select column_name(s)  from table1;
```

**insert into select 一般用于复制表数据**，步骤如下：

**复制表数据，方式一：** 

第一步，查询目标表的表结构，获取创建该表的完整DDL语句：

```sql
show create table student;
```

第二步，修改表名称，运行DDL语句创建一个新表：

```sql
CREATE TABLE `copy_student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `version` varchar(255) DEFAULT NULL,
  `hometown` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `version` (`version`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=24 DEFAULT CHARSET=utf8
```

第三步，使用 insert into select 复制源表数据到目标表：

```sql
insert into copy_student select * from student ;
```

此时创建的复制表，无论是表结构还是表数据，除了表名称不同，其他内容全部一样。

**复制表数据，方式二：** 

当然，如果只想复制表数据，可以通过一条语句直接搞定：

```sql
create table copy_aim_table
as
select * from aim_table;
```

​	这样，创建新表和复制表数据就一步到位了，源表的部分表结构和全部表数据就复制过来了。**注意，通过后面这种方法，不会把源表的与表结构相关的属性复制过来，如主键、索引、自增约束、注释等**，这些信息不会复制过来，所以 `create table as select` 只能做表数据的复制，不能复制表结构全部信息，如果想深度复制表，使用第一种方法即可。

注：

​	MySQL不支持 select into from 语句，可用 insert into select 代替；



### delete from

**单表删除**

```sql
delete from t1 where t1.id=3;
```

**连表删除**

```mysql
#  t1,t2连表,只删除t1表中的记录

delete t1 
from t1 left join t2 on t1.recid = t2.mrecid
where
t1.id=3;

# t1,t2连表,删除t1表和t2表中的记录
delete t1,t2 
from t1 left join t2 on t1.recid = t2.mrecid
where
t1.id=3;
```

**注意** 

​	delete 语句中，不能给表名起别名！即 t1,t2 不能是别名。



### truncate table

**说明**

​	清空表。

**语法**

```sql
truncate table table_name;
```

`truncate` 和 `delete` 的区别:

1. truncate 直接删除整张表的所有数据。delete 是逐条删除数据，可根据条件选择删除；
2. truncate不可以回滚事务，delete可以回滚；
3. truncate 清空表的效率更高；



### update

update 语句用于更新表数据。

**单表更新** 

```sql
update student as s 
set s.age = 18,s.name = "LiLi"
where 
s.id=22 and version = 10020;
```

注:

- update 语句如果不指定where 条件，则更新全表数据；
- 多数情况下都会指定where条件进行更新，where条件的字段如果都没有设置索引，那么执行更新期间会锁全表；
- 建议update语句的where条件至少有一个设置索引，这样不至于锁住全表，提高并发效率；

特别注意:

​	**update语句的多列更新，必须使用英文逗号隔开，不要使用 `AND` 符号，尽管这么做很多情况下都不会报错并且执行成功，但执行结果却是完全不一样的！**如上面的例子如果写成 `s.age = 18 AND s.name = "LiLi"` ，就会给 s.age 设置一个Boolean值，表达式是 `18 AND s.name = "LiLi"` ，如果为true 返回1，false返回0。

**连表更新** 

```mysql
UPDATE bms_md_goods AS g
JOIN bms_md_stock AS m
ON g.id = m.GOODS
SET g.stock_num = m.TOTAL_NUM,
g.total_price = m.TOTAL_PRICE;
```

注：

​	连表更新和连表的方式有直接关系，如果是内连接，则只可能更新两个表共同匹配的部分，如果是左连接，在没有where条件的情况下会更新左表的所有行。



## 2.2 DDL 数据定义语言

​	通过数据定义语言DDL（Data Definition Language），可以实现对数据库中数据结构的增删改查操作，DDL 涉及的语句有 CREATE、DROP、ALTER、SHOW 。

### CREATE

**语法** 

```mysql
# 创建数据库
create database database_name [default charset utf8];
# 创建表
create table table_name;	
# 创建索引
create index index_name ON table_name (column...)
```

栗子:

```mysql
CREATE TABLE `member_points_rule` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `version` bigint(20) DEFAULT NULL COMMENT '版本号',
  `create_time` timestamp NULL DEFAULT NULL COMMENT '创建时间',
  `create_user` varchar(100) DEFAULT NULL COMMENT '创建人',
  `last_modify_time` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '最后修改时间',
  `last_modify_user` varchar(100) DEFAULT NULL COMMENT '最后修改人',
  `rule_name` varchar(100) DEFAULT NULL COMMENT '规则名称',
  `rule_code` varchar(100) DEFAULT NULL COMMENT '规则编码',
  `rule` varchar(255) DEFAULT NULL COMMENT '规则说明 10,1 (每消费10元获取1个积分)',
  `use_range` varchar(255) DEFAULT NULL COMMENT '使用范围',
  `validity` int(11) DEFAULT NULL COMMENT '有效期(天)',
  `memo` varchar(255) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 ROW_FORMAT=COMPACT COMMENT='会员积分规则主表'
```



### DROP

**语法** 

```mysql
# 删除数据库
drop database dbname;	
# 删除表
drop table table_name;	
# 删除索引
drop index index_name on table_name;
```

:warning: 特别注意:

​	删库删表需要特别谨慎！在生产环境下几乎不会进行删库删表操作。

### ALTER

**说明** 

​	`alter table` 用于修改现有的表结构。

**语法** 

```mysql
# 添加列
alter table table_name 
	add column_name datatype,
	add column_name2 datatype;
# 删除列
alter table table_name 
	drop column_name;
# 修改列
alter table table_name 
	change column_name column_new_name datatype;	
# 修改表名
alter table table_name 
	rename table_new_name;
```

栗子：

```mysql
alter table member_rule 
	add `use_level` varchar(100) DEFAULT NULL COMMENT '使用等级(多选)',
 	add `startflag` bit(1) DEFAULT b'0' COMMENT '启用标识';
```



### SHOW

```mysql
# 查看数据库中所有的表
SHOW tables;
# 查看指定表的字段信息;
SHOW COLUMNS FROM student;
# 查看表创建语句
SHOW create table table_name;
# 查看索引
show index from table_name;	
# 查看所有会话级系统变量;(默认查看的是会话级别变量)
SHOW (SESSION) VARIABLES;
# 查看所有全局级系统变量
SHOW GLOBAL VARIABLES;	
# 查看系统变量支持模糊查询
SHOW VARIABLES like '%INNODB%';	
```



## 2.3 DCL 数据控制语言

​	DCL （Data Control Language）语句可以对数据库的访问权限进行控制，由 `GRANT` 和 `REVOKE` 两个指令组成。 数据库权限管理在实际项目中是由数据库管理员（DBA）专门控制的，所以一般开发人员很少去使用DCL 语句来操作数据库权限。 



## 2.4 约束

​	约束即限制的意思，给表或表中的字段添加约束，从而限制用户操作数据表的行为。

​	从约束的作用对象上可分为表级约束和列级约束，添加在表上的约束为表级约束，添加在列上的约束称为列级约束。

- 表级约束
- 列级约束



Mysql有如下几种约束行为：

- 默认值约束
- 非空约束
- 唯一约束
- 主键约束
- 外键约束
- 自增长约束



### 默认值约束(DEFAULT)

插入数据时，如果不给该字段赋值，将会赋默认值：

```mysql
`RECVER` bigint(20) DEFAULT '0'
```

### 非空约束（NOT NULL ）

规定字段值不能为空：

```mysql
`RECID` binary(16) NOT NULL
```

### 唯一约束（UNIQUE）

唯一约束强制列值唯一，插入、修改时会做重复性检查，字段值不可以重复。

```mysql
`RECID` binary(16) UNIQUE
```

​	**特别注意，唯一约束是借助唯一索引实现的。**从概念上讲，约束和索引是完全不同的概念，唯一约束仅仅是要求列值必须唯一，理论上讲做一次全表扫描并判断就可实现，但MySQL为了提高性能，唯一约束使用了唯一索引来提高效率，所以尽管叫法不同，但唯一约束和唯一索引在MySQL中是一样的。

​	当你为某个表字段创建唯一约束时，MySQL自动创建唯一索引。

​	唯一约束、唯一索引允许字段值为空；当表字段已有重复值时，此时无法创建唯一约束，必须保证没有重复值才能创建成功；

### 主键约束（PRIMARY KEY）

主键是非空、唯一的，MySQL的一个表只能有一个主键，主键可以是单列主键，也可以是多列联合主键：

```mysql
# 单列主键
PRIMARY KEY (`RECID`)
# 联合主键
PRIMARY KEY (`id`,`code`)
```

​	这里需要额外注意：主键是非空、唯一的，但与只是非空、唯一的列有本质的不同，主键会自动添加索引，并且这个索引也与普通索引不同，主键索引是聚簇索引。

### 外键约束（FOREIGN KEY）

外键用于关联两个表，不只是关联两个表的数据，还有删除和更新的联动操作和行为约束。

在创建外键的时候，要求父表的对应列上必须有索引，还要设置在父表删除、更新时，子表如何联动处理；

联动处理有4个可选值：

- RESTRICT
- NO ACTION
- SET NULL
- CASCADE

其中在MySQL中， RESTRICT 和 NO ACTION 相同，是指父子表在有关联记录的情况下父表不能更新和删除；

SET NULL 是指父表更新，子表外键设置为Null，如果此时外键有非空约束，就会报错；

CASCADE 是指父表更新、删除，子表同样进行更新和删除，严格保持数据一致性；

语法：

```mysql
CONSTRAINT `orders2_oid` 
FOREIGN KEY (`oid`) REFERENCES `orders` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
```

​	 `CONSTRAINT` 用来指定外键名称，`FOREIGN KEY` 指定外键字段，`REFERENCES` 指定父表和字段，`ON DELETE` 指定联动删除的操作，`ON UPDATE` 指定联动更新的操作。

### 自增长约束（AUTO_INCREMENT）

语法：

```mysql
`id` int(11) AUTO_INCREMENT
```

注：一个表最多只能有一个自增长约束！

查看表的下一个自增长值: 

```mysql
# 方式一:
SHOW CREATE TABLE table_name;
# 方式二:
SELECT AUTO_INCREMENT from INFORMATION_SCHEMA.TABLES where TABLE_NAME='table_name';
```



### 注释（COMMENT）

使用 COMMENT 给表或列添加注释:

```mysql
# 给列添加注释
`age` int(11) COMMENT '年龄'
# 给表添加注释
CREATE TABLE `member_card` (
    ...
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='会员卡'
```

注：给数据表添加描述业务的注释是非常好的习惯，方便团队合作开发，提高开发和运维效率。

## 2.5 SQL分析

### 查询剖析-EXPLAIN

`EXPLAIN` 命令用法很简单, 在 SELECT 语句前加上 `EXPLAIN` 关键字就可以了:

```mysql
EXPLAIN 
SELECT * from user_info WHERE id < 300;
```

`EXPLAIN` 命令输出的各列内容如下:

- id: SELECT 查询的标识符. 每个 SELECT 都会自动分配一个唯一的标识符
- select_type: SELECT 查询的类型
- table: 查询的是哪个表
- type: join 类型
- possible_keys: 此次查询中可能选用的索引
- key: 此次查询中确切使用到的索引
- ref: 哪个字段或常数与 key 一起被使用
- rows: 显示此查询一共扫描了多少行. 这是一个估计值
- extra: 额外的信息



### type

type 常用的取值有:

- `system`: 表中只有一条数据， 这个类型是特殊的 `const` 类型。

- `const`: 针对主键或唯一索引的等值查询扫描， 最多只返回一行数据。 const 查询速度非常快，因为它仅仅读取一次即可。

- `eq_ref`: 此类型通常出现在多表的 join 查询, 表示对于前表的每一个结果, 都只能匹配到后表的一行结果. 并且查询的比较操作通常是 `=`， 查询效率较高。

- `ref`: 此类型通常出现在多表的 join 查询, 针对于非唯一或非主键索引, 或者是使用了 `最左前缀匹配` 规则索引的查询。

- `range`: 表示使用索引范围查询，通过索引字段范围获取表中部分数据记录。这个类型通常出现在 =, <>, >, >=, <, <=, IS NULL, <=>, BETWEEN, IN() 操作中。

- `index`: 表示全索引扫描， 和 ALL 类型类似，只不过 ALL 类型是全表扫描，而 index 类型则仅仅扫描所有的索引，而不扫描数据。
  `index` 类型通常出现在：所要查询的数据直接在索引树中就可以获取到， 而不需要扫描数据。 当是这种情况时， Extra 字段 会显示 `Using index` 。

- `ALL`: 表示全表扫描， 这个类型的查询是性能最差的查询之一。 

  通常来说，我们的查询不应该出现 ALL 类型的查询， 因为这样的查询在数据量大的情况下，对数据库的性能是巨大的灾难。 如一个查询是 ALL 类型查询， 那么一般来说可以对相应的字段添加索引来避免。



**type 类型的性能比较**

通常来说, 不同的 type 类型的性能关系如下:

​	 `ALL < index < range < ref < eq_ref < const < system`

​	 `ALL` 类型因为是全表扫描, 因此在相同的查询条件下, 它是速度最慢的。而 `index` 类型的查询虽然不是全表扫描, 但是它扫描了所有的索引, 因此比 ALL 类型的稍快。后面的几种类型都是利用了索引来查询数据, 因此可以过滤部分或大部分数据, 因此查询效率就比较高了。

【建议】

​	SQL性能优化的目标：至少要达到 `range` 级别，要求是 `ref` 级别，最好是 `const` 。



### key

​	此字段是 MySQL 在当前查询时所真正使用到的索引。

### key_len

​	表示查询优化器使用了索引的字节数。这个字段可以评估组合索引是否完全被使用， 或只有最左部分字段被使用到。
key_len 的计算规则如下:

字符串

- char(n): n 字节长度
- varchar(n): 如果是 utf8 编码, 则是 3 n + 2 字节; 如果是 utf8mb4 编码, 则是 4 n + 2 字节。

数值类型:

- TINYINT: 1字节
- SMALLINT: 2字节
- MEDIUMINT: 3字节
- INT: 4字节
- BIGINT: 8字节

时间类型

- DATE: 3字节
- TIMESTAMP: 4字节
- DATETIME: 8字节

### rows

​	rows 也是一个重要的字段， MySQL 查询优化器根据统计信息，估算 SQL 要查找到结果集需要扫描读取的数据行数。
​	这个值非常直观显示 SQL 的效率好坏， 原则上 rows 越少越好。

## 2.6 MySQL与标准SQL的差异

​	首先说一下标准SQL，其实你可以不用去理会什么是标准SQL，事实上，大多数人也不需要去学习标准SQL的规范，标准SQL只是行业内的一个规范，技术在不断发展，规范也就一直在变化。

​	MySQL的语法和标准SQL语法几乎一样，只有几处是有所区别的。

### 选择表的差异

mysql不支持 SELECT ... INTO TABLE 语法，可以使用 INSERT INTO ... SELECT 语句代替



### 更新差异

mysql中是按照列表达式执行的先后顺序赋值的。下面两个例子列表达式的先后顺序不一样，执行结果也不一样。

```mysql
# 例1
UPDATE t1 SET col1 = col1 + 1,col2 = col1;
# 例2
UPDATE t1 SET col2 = col1,col1 = col1 + 1;
```

​	例1中col2 更新的是col1的最新值，而不是原值，执行结果col2 = col1；例2中col2 更新的是col1的原值，不是最新值，执行结果col2 != col1。



# 3. 数据类型（Data Type）UGtdySjuV

数据表的字段都需要指定名称和数据类型，Mysql的数据类型如下：

1. 数值型
2. 字符型
3. 日期时间型
4. 二进制型



## 3.1 数值型



**整数**

| 类型      | 大小(字节) | 范围（有符号）                  | 范围（无符号）     | 用途     |
| --------- | ---------- | ------------------------------- | ------------------ | -------- |
| TINYINT   | 1          | (-128，127)                     | (0，255)           | 极小整数 |
| SMALLINT  | 2          | (-32 768，32 767)               | (0，65 535)        | 小整数   |
| MEDIUMINT | 3          | (-8 388 608，8 388 607)         | (0，16 777 215)    | 中等整数 |
| INT       | 4          | (-2 147 483 648，2 147 483 647) | (0，4 294 967 295) | 整数     |
| BIGINT    | 8          | (-9 +18位 , 9 +18位)            | (0，18 +18位)      | 大整数   |

**小数**

| 类型         | 大小(字节) | 范围（有符号） | 范围（无符号） | 用途         |
| ------------ | ---------- | -------------- | -------------- | ------------ |
| FLOAT        | 4          |                |                | 单精度浮点数 |
| DOUBLE       | 8          |                |                | 双精度浮点数 |
| DECIMAL(M,D) | M+2        | 取决于M和D的值 | 取决于M和D的值 | 严格定点数   |

注：

​	存储小数类型务必使用 decimal ，禁止使用 float 和 double ！

​	在存储的时候， float 和 double 存在精度损失的问题，很可能在比较值的时候，得不到正确的结果。如果存储的数据范围超过 decimal 的范围，可以将数据拆分为整数部分和小数部分分开存储。




## 3.2 字符型

**字符**

| 类型       | 大小(字节) | 用途         |
| ---------- | ---------- | ------------ |
| CHAR(N)    | 0~255      | 定长字符串   |
| VARCHAR(N) | 0~65535    | 可变长字符串 |

**文本** 

| 类型       | 大小(字节)             | 用途       |
| ---------- | ---------------------- | ---------- |
| TINYTEXT   | 0~255（256b）          | 短文本     |
| TEXT       | 0~65535（64Kb）        | 文本       |
| MEDIUMTEXT | 0-16 777 215（16Mb）   | 中长度文本 |
| LONGTEXT   | 0-4 294 967 295（4Gb） | 超长文本   |



注:

​	N 表示字符的长度，每个字符型的大小确定，但根据字符编码不同，所存储的字符长度是不同的：

- **UTF8**：一个汉字占用3个字节
- **UTF8mb4**： 一个汉字占用4个字节
- **GBK**： 一个汉字占用2个字节

​	例如 VARCHAR(n) ，最大存储65535 个字节，如果是utf8编码，n 的最大值为 65535 / 3 = 21845 ；而 CHAR(n) 中 n 的最大值为 255 / 3 = 85  。

​	4 种 TEXT 类型：TINYTEXT、TEXT、MEDIUMTEXT和 LONGTEXT，它们区别在于可存储的最大长度不同。



## 3.3 日期时间型



| 类型      | 大小(字节) | 范围          | 格式                | 用途             |
| --------- | ---------- | ------------- | ------------------- | ---------------- |
| DATE      | 3          |               | YYYY-MM-DD          | 日期值           |
| TIME      | 3          |               | HH:MM:SS            | 时间值或持续时间 |
| DATETIME  | 8          | 1001年-9999年 | YYYY-MM-DD HH:MM:SS | 日期时间值       |
| TIMESTAMP | 4          | 1970年-2038年 |                     | 时间戳           |

注：

​	MySQL的时间类型最高精确到**秒**，无法直接精确到毫秒或微秒。如果想记录毫秒或微秒，可以使用整数类型单独存储。

​	`DATETIME` 和 `TIMESTAMP` 类型有一个很方便的特性：**更新数据时自动赋值当前时间**。可用于记录数据最后的修改时间，非常实用。DATE 和 TIME 时间类型则没有此特性。

```mysql
`last_modify_time` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '最后修改时间'
```

​	DATETIME 类型按照   YYYYMMDDHHMMSS 整数形式存储值，与时区无关。

​	TIMESTAMP 保存了从1970-01-01 （格林尼治标准时间）以来的秒数，这和UNIX 时间戳相同。TIMESTAMP 只使用4个字节保存值，因此表示的时间范围比 DATETIME 小得多，只能表示 1970年-2038年范围的时间，TIMESTAMP 存储的值与时区有关系。



## 3.4 二进制型



**位** 

| 类型 | 大小(位) | 用途                       |
| ---- | -------- | -------------------------- |
| BIT  | 0~64     | 记录布尔值或分类很少的类型 |

**二进制** 

| 类型         | 大小(字节) | 用途               |
| ------------ | ---------- | ------------------ |
| BINARY(n)    | 0~255      | 定长二进制字符串   |
| VARBINARY(n) | 0~65535    | 可变长二进制字符串 |

**BLOB** 

| 类型       | 大小(字节)             | 用途           |
| ---------- | ---------------------- | -------------- |
| TINYBLOB   | 0~255（256b）          | 短二进制       |
| BLOB       | 0~65535（64Kb）        | 二进制         |
| MEDIUMBLOB | 0-16 777 215（16Mb）   | 中长度二进制   |
| LONGBLOB   | 0-4 294 967 295（4Gb） | 超大二进制数据 |

注:

​	BINARY 和 VARBINARY 是二进制字符串，与 CHAR 和 VARCHAR 类似。

​	它们没有字符集，排序和比较基于列值字节的数值。

​	4 种 BLOB 类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。它们区别在于可容纳存储范围不同。



# 4. 函数（Function）UGtdySjuV



## 4.1 内置函数

MySQL 提供了很多内置函数。

### 数学函数

```mysql
ROUND(1234567.895,2)		# 四舍五入; 1234567.90
TRUNCATE(1234567.895,2)		# 将小数指定位置后面的值直接抹去;	1234567.89
FORMAT(1234567.895,2)		# 格式化数字，整数部分从低到高每3位按逗号分隔，小数部分保留并四舍五入: 1,234,567.90
ABS(x)						# 计算绝对值
HEX(x)						# 转换为十六进制并返回
BIN(x)						# 将数字转换为二进制形式并返回
```

聚合函数:

```mysql
COUNT(*)			# 统计表的数据行数
MIN(col)			# 计算表达式最小值
MAX(col)			# 计算表达式最大值
AVG(col)			# 计算平均值
SUM(col)			# 求和
GROUP_CONCAT(col)	# 将属于一组的列值以字符串拼接的形式返回
```

注:	

- count(col) 统计的是非空列的记录数；
- count(*) 统计的是该表所有的记录数；
- count(distinct col) 统计的是非空且不重复的记录数 ；




### 日期函数

常用日期处理函数：

```mysql
NOW();					# 当前日期时间值
CURDATE();				# 当前日期值
DATE(d);				# 返回指定日期的日期部分

DATE_FORMAT(NOW(),'%Y-%m-%d %T');					# 日期转换成字符串
STR_TO_DATE('2018-08-08 08:00:00','%Y-%m-%d %T') 	# 字符串转换为日期

ADDDATE(NOW(),2);		# 加上n天的日期时间值
SUBDATE(NOW(),2);		# 减去n天的日期时间值
DATEDIFF(d1,d2);		# 计算两个日期的差值: d1-d2

DAYOFYEAR(d);			# 计算日期是本年第几天
DAYOFMONTH(d);			# 计算日期是本月第几天
DAYOFWEEK(d);			# 计算日期是周几:1为周日,2为周一...
```



### 字符函数

常用字符处理函数：

```mysql
# 去除字符串左右两侧的空格
TRIM(str)
# 替换函数,使用s2替换字符串str中所有的子字符串s1
REPLACE(str,s1,s2)		 
# 获取子字符串,从str的pos位置开始,获取len长度的字符并返回
SUBSTRING(str,pos,len)	 
# 计算字符串字符个数并返回
CHAR_LENGTH(str)				 
# 计算字节长度并返回
LENGTH(str)  
# 拼接多个字符串,并返回
CONCAT(str1,str2, ...) 	
# 匹配子字符串开始位置,返回s1在str中第一次出现的位置
LOCATE(s1,str)					 
# 将字符串中的字母转换成小写,并返回
LCASE(str) 						 
# 将字符串中的字母转换成大写,并返回
UCASE(str)						 
# 截取左侧字符串函数,返回str最左边len个字符
LEFT(str,len)	
# 截取右侧字符串函数,返回str最右边len个字符
RIGHT(str,len)	
```



### 加密函数

常用加密函数：

```mysql
ENCODE(str,pswd_str);			# 使用pswd_str作为密码，加密str
DECODE(crypt_str,pswd_str);		# 使用pswd_str作为密码，解密crypt_str(已加密字符串)
MD5(str);						# MD5散列函数,计算出128位比特结果,以32位十六进制数表示
SHA1(str);						# SHA1 散列函数,计算出20个字节长度的结果,以40个十六进制数的形式表示
```



### 流程控制函数

常用流程控制函数：

```mysql
IF(expr1,expr2,expr3)	# 如果表达式 expr1为真,返回expr2,否则返回expr3;
IFNULL(expr1,expr2)		# 如果 expr1 不为 NULL,返回expr1,否则返回expr1;
COALESCE (expr, ...) 	# 返回第一个不为 NULL 的值,常用于将 NULL 值转化为0;

# 条件判断语句: CASE WHEN
CASE expression
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE result
END
```

​	CASE 表示函数开始，END 表示函数结束。如果 condition1 成立，则返回 result1, 如果 condition2 成立，则返回 result2，当全部不成立则返回 else 结果，而当有一个成立之后，后面的就不执行了。

### 系统信息函数

```mysql
DATABASE();			# 返回当前数据库名
VERSION();			# 数据库版本号
USER();				# 返回当前操作用户
CONNECTION_ID();	# 返回当前用户的连接ID
```



## 4.2 自定义函数

MySQL支持用户自定义函数。

### 语法

```mysql
create function 函数名(参数列表) 
	returns 数据类型
begin
 sql语句;
 return 数据;
end;
```

### 示例

```mysql
# 示例1: 入参是int(11)类型,返回值是varchar(50)类型
CREATE DEFINER=`root`@`localhost` FUNCTION `get_stu_name`(id int(11)) RETURNS varchar(50) CHARSET utf8
BEGIN
	declare result varchar(50);
	select s.`name` from student as s where s.id = id into result;
	RETURN result;
END

# 示例2
CREATE DEFINER=`root`@`localhost` FUNCTION `dna_collate_gbk`(s varchar(2000)) RETURNS varbinary(4000)
begin
	declare i int default 1;
	declare c varchar(4);
	declare r varbinary(4000) default binary '';
	declare t binary(2);
	while i <= char_length(s) do
		set c = substr(s, i, 1);
		if ascii(c) < 128 then 
			set r = concat(r, dna_byte2hex(0), dna_byte2hex(ascii(c)));
		else
			select sn into t from CORE_COLLATE_GBK where ch = c;
			set r = concat(r, t);
		end if;
		set i = i + 1;
	end while;                  
	return r;
end
```

### 流程控制语句

MySQL定义了一组控制程序的语法，包括但不限于变量声明与赋值、条件判断、循环语句等。



**变量声明与赋值** 

```mysql
# 声明局部变量
declare c varchar(10);
# 赋值局部变量
set c = concat(r, t);
# 声明局部变量并赋默认值
declare i int default 1;
# 使用into将查询结果赋值给变量 
select s.`name` from student as s where s.id = id into result;
```

**条件判断语句** 

```mysql
# if 条件判断
if (val is null) then
	return '';
elseif (val <16) then
	return concat('0', hex(val));
else
	return hex(val);
end if;

# case when 判断 (用法1)
case var
    when 0 then
      insert into t values(17);  
    when 1 then
      insert into t values(18);  
    else
      insert into t values(19);  
end case;  
# case when 判断 (用法2)
case
    when var > 0 then
      insert into t values(17);  
    when var < 0 then
      insert into t values(18);  
    else
      insert into t values(19);  
end case;
```

case when 判断 有两种用法，用法一是以枚举的形式列出所有等于表达式的情况，用法二没有case 表达式，用起来更像if判断语句。

**循环语句** 

```mysql
# while 循环
while i <= char_length(s) do
	insert into t values(v);  
	set i = i+1; 
end while;
# loop 循环
LOOP_LABLE:loop  
	insert into t values(v);  
	set v=v+1;  
	if v >=5 then 
		leave LOOP_LABLE;  
	end if;  
end loop; 
```

loop 循环不需要初始条件，leave 代表离开循环。



**查看自定义函数:** 

```mysql
# 查看数据库所有自定义函数
SHOW FUNCTION STATUS;
# 查看指定数据库所有自定义函数
SHOW FUNCTION STATUS WHERE db = db_name;
# 按函数名模糊查询
SHOW FUNCTION STATUS LIKE '%date%';
# 查看指定自定义函数的详情信息
SHOW CREATE FUNCTION func_name;  	
```

注：查看自定义函数和存储过程的语法完全一模一样，只不过关键字不同。

### 函数和存储过程

​	函数（function）和存储过程（procedure）在功能上比较相似，都是对一组逻辑的封装，目的是可以重复使用。但两者也有很多区别，函数必须使用return 进行返回，而存储过程不需要；二者的使用场景也不一样，函数一般封装成公用的工具方法，可以在SQL中直接使用，比如比较日期、格式转换等；存储过程则是封装业务代码，如业务数据重算，库存异常数据扫描等，需要业务服务器单独调用存储过程。



# 5. 视图（View）UGtdySjuV

​	视图是虚拟表，是从其他物理表或视图中导出的表，数据库只存放视图的定义，不存放视图的数据。

​	视图是MySQL服务层实现的功能。

## 5.1 作用

1. 权限控制使用；
2. 简化复杂查询；

​	对于一个数据表，如果只想让用户操作部分字段，而不是全部字段，就可以使用视图，通过视图自定义查询结果列，从而到达权限控制的作用；

​	简化复杂查询，使用创建临时表的方式也可以做到，不过临时表只能在本次会话中生效，作用范围有限，如果临时表数据经常反复使用或多个地方需要引用，创建一个视图是个不错的方法，视图的作用范围是全局的。

## 5.2 使用视图



```mysql
# 创建视图
create view view_name as 
select column_name(s) from table_name where condition;
# 删除视图
drop view view_name;
# 修改视图
alter view view_name as 
select statement;
# 根据视图查询数据
select * from view_name;
```

:warning: 特别注意：

​	查询视图总是查询最新数据！

​	如果视图和物理表的字段是一一对应的，那么是可以通过操作视图对数据进行增删改的！如果视图的字段是通过计算得出的结果，则无法通过视图修改。



# 6. 索引（Index）UGtdySjuV

## 6.1 索引目的

索引的主要目的在于提高查询效率。另外，InnoDB的行锁和间隙锁也是通过索引实现的。

## 6.2 索引类型

​	InnoDB表的索引从数据组织层面可分为两类：

1. **聚簇索引**（一般是主键索引）
2. **辅助索引**（二级索引）



从约束的层面分析，分为如下几类：

### 普通索引

​	普通索引（INDEX）：最基本的索引，它没有任何限制，用于加速查询。

```mysql
KEY `version` (`version`) USING BTREE
```

### 唯一索引

​	唯一索引（UNIQUE ）：索引列的值必须唯一，允许有空值。如果是组合索引，则列值的组合必须唯一。

```mysql
UNIQUE KEY `version` (`version`) USING BTREE
```

### 主键索引

​	主键索引（PRIMARY KEY）：一个表只能有一个主键（非空、唯一），可以是联合主键。一般是在建表的时候同时创建主键索引。

​	主键索引和加非空、唯一约束的索引有本质的区别：主键索引是聚簇索引，只是加了非空、唯一约束的索引是非聚簇索引（二级索引），二者组织数据的形式不同。

```mysql
PRIMARY KEY (`id`)
```

特别注意:

- 主键强烈推荐递增式的插入数据；
- 主键的长度不应过长，过长的主键会影响索引搜索效率，也会占用过多的磁盘空间；



### 组合索引

​	指多个字段上创建的索引，只有在查询条件中使用了创建索引时的第一个字段，索引才会被使用。使用组合索引时要遵循**最左前缀匹配原则**。

```mysql
KEY `home` (`hometown`,`amount`) USING BTREE
```

​	组合索引可以是聚簇索引（联合主键），也可以是辅助索引（二级索引）。 

**最左前缀匹配原则**：

​	最左前缀匹配原则是数据库索引文件(B树索引)的特性，即最左边的值一定要确定下来，索引才能生效。最左原则的顺序不是指查询条件中的顺序, 而是创建的索引列顺序。

​	如上面的 key（(`hometown`,`amount`) ）这个组合索引，查询条件中一定要包含最左侧的 hometown 字段，才有可能使用索引查询，但是查询条件中的顺序是没有要求的，因为mysql优化器会对顺序进行优化。



在以下两个应用场景需要遵守：

1. 使用组合索引时，最左列的索引一定要用到（查询条件顺序无所谓）。
2. 右模糊查询（LIKE语句）才能使索引生效。左模糊和全模糊均不能使索引生效。



注意:

- 组合索引不满足最左前缀匹配原则, 不会使用索引。
- 组合索引的顺序非常重要。根据经验，应该把使用最频繁、区分度高的一列放在组合索引的最左边。
- 如果组合索引中第一个字段是范围查询需要单独建一个索引。
- 组合索引中包含的字段个数不应该太多，建议不要超过5个。



### 全文索引

全文索引（FULLTEXT）：对文本的内容进行分词，进行全文搜索。

```mysql
FULLTEXT KEY `home` (`hometown`)
```



### 限制索引长度

​	前面提到主键的长度不要过长，因为主键也是一个索引，而对于普通索引，如时间、字符串类型的列，也不应该过长。

#### 语法

```mysql
KEY `version` (`version`(16)) USING BTREE
```

上面的 `version` 列是字符类型的，创建索引，并且指定索引长度为16。

#### 优点

​	限制索引长度的优点是节省磁盘空间，不至于索引文件过大，提高数据插入效率。



#### 缺点

​	限制索引长度的缺点也不少，主要在于两点：

1. 如果对主键或唯一索引限制索引长度，那么插入唯一数据时，有可能会插入失败！原因很简单，长度是18的时候，数据是唯一的，那截取为16长度的时候，不能保证数据是唯一的，如果截取后的数据重复了，MySQL就会报错！
2. 使覆盖索引查询不再有效，如果查询的列中在索引中都可以找到，那么就不需要回表查询，大大提高查询效率，可当限制索引长度后，索引文件上的数据并不一定是全部的原表数据，而是截取后的部分数据，所以即便是按照覆盖索引查询的规范去写查询，也不会生效了，而是再回表查询原表数据，多了很多操作，降低了效率。



​	综上所述，限制索引长度虽然有优点，但弊端还是不容忽视的，如果想要限制索引长度，需慎重分析，然后考虑到底要不要限制。



## 6.3 查看索引

通过 `show index` 查看一张表的索引信息：

```mysql
SHOW INDEX FROM table_name;
```

输出:

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200303213104386.png" alt="image-20200303213104386" style="zoom:67%;" />

## 6.4索引优化

### 适用场景

​	B-Tree 索引适用于**键全值查询，键范围查询**和**键左前缀查询**。

1. 全键值匹配

   全键值匹配指的是和索引中的所有列进行匹配。例如是组合索引（name，age），就查找姓名为王三，年龄为25的人。

2. 匹配最左前缀

   只是用索引的第一列。例如是组合索引（name，age），只查找姓名为王三的人。

3. 匹配列前缀

   只匹配某一列的值的开头部分。例如查找姓王的所有人。（注意，左前缀必须明确，索引才有效！）

4. 匹配范围值

   例如对于单列索引（age），查找年龄在18~60岁之间的人。

5. 精确匹配某一列并范围匹配另一列

   例如查找名字为王三，年龄在18~60岁之间的人。

6. 只访问索引的查询（覆盖索引）

   B-Tree 索引支持“只访问索引的查询”，即查询只访问索引，不回表访问数据行。



### B-Tree 索引的限制

下面的场景全部都无法使索引生效：

1. 对于组合索引，如果查询条件**不包含**最左列索引，则无法使用索引。

2. **左模糊和全模糊**无法使用索引，对于匹配列前缀，左边部分的内容必须确定，即索引支持右模糊查询，但不支持左模糊和全模糊查询。
3. 如果查询中有某个列的范围查询，则其右边的所有列都无法使用索引。
4. 如果索引列是**运算表达式**的一部分，或是函数的参数，索引无效。
5. 数据出现**隐式类型转换**的时候不会命中索引，特别是当列类型是字符串，一定要将字符常量值用引号引起来。
6. 使用否定条件：`<> 或 !=、NOT IN、IS NOT NULL`，否定条件**通常**会使索引失效，从而进行全表扫描。



注：

​	场景一，对于组合索引（name，age），直接查找年龄在18~60岁之间的人，是全表扫描，不会使用索引。

​	场景三，右边的列指的是表定义结构中列的左右顺序，不是where 条件中的顺序。



下面是索引效率不高的应用场景：

1. 设置索引的列的区分度应该很高，即该列的值重复率不要太高，比如性别、状态之类的枚举类型，即便使用索引，效率提升也不大。




**适用范围**

- 对于很小的表，比如只有几十行或几百行数据，没必要使用索引，全表扫描更高效。
- 对于中到大型的表，索引就非常有效。
- 对于特大型的表，比如上千万条数据，使用索引的代价会越来越大，此时需要考虑使用分区或分表。



### 高性能索引策略



1. **符合最左前缀匹配原则【非常重要的原则】。**

   mysql会一直向右匹配直到遇到范围查询(>、<、between、like)就停止匹配，比如a = 1 and b = 2 and c > 3 and d = 4 如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，a,b,d的顺序可以任意调整。

2. **尽量选择区分度高的列建立索引。**

   区分度也叫选择性、基数，唯一键的区分度是1，而一些状态、性别字段在大数据面前区分度近乎为0，区分度较小的键，索引效果较差；

   区分度的公式是 `count(distinct col)/count(*)` ；

3. **在varchar类型字段上建立索引时，必须指定索引长度；**

   长度和区分度之间做一个衡量即可。

4. **索引列不能参与计算，保持列“干净”。**

   比如from_unixtime(create_time) = ’2014-05-29’就不能使用到索引；同样的，where age +1 > 10也不能使用到索引，原因很简单，b+树中存的都是数据表中的字段值，但进行检索时，需要把所有元素都应用函数才能比较，显然成本太大。所以语句应该写成create_time = unix_timestamp(’2014-05-29’)。

5. **单张表的索引个数不要过多，一般建议不要超过5个。**

   索引需要额外的磁盘空间，并降低写操作的性能。在修改表内容的时候，索引会进行更新甚至重构，索引列越多，这个时间就会越长。所以只保持需要的索引有利于查询即可。
   
6. **组合索引中包含的字段数不要过多，一般建议不要超过5个。 ** 

7. **主键的插入一定是递增的。**（提高B+树数据结构的分页效率）

   


### B-Tree 索引的优点

​	索引可以让服务器快速的定位到表的指定位置，这是索引最主要的作用，但这不是索引唯一的作用，索引还有一些其他的附加作用。

​	B-Tree 索引是按照顺序存储数据，所以Mysql可以用来做 ORDER BY 和 GROUP BY 操作。因为数据是有序的，所以B-Tree 也就将相邻的列值都存储在一起。最后，因为索引存储了实际的列值，所以某些查询只使用索引就能够完成全部查询。

​	据此特性，总结出索引有如下三个优点：

1. 索引大大减少了服务器需要扫描的数据量。
2. 索引可以帮助服务器避免排序和使用临时表。
3. 索引可以将随机I/O变为顺序I/O。



《阿里巴巴Java开发手册》索引规约

**【强制】**

1. 【强制】业务上具有唯一特性的字段，即使是多个字段的组合，也必须建成唯一索引。

   唯一索引会明显提高查询速度。

2. 【强制】在varchar字段上建立索引时，必须指定索引长度。

   没必要对全字段建立索引，根据实际文本的区分度决定索引长度即可。（一般字符串类型长度为20的索引，区分度高达90%以上）

3. 【强制】模糊搜索like 严禁左模糊或全模糊，使用右模糊。如果需要可以使用搜索引擎来解决。

   索引文件具有B树的最左前缀匹配特性，如果左边的值未确定，那么无法使用索引。

   同样的，使用组合索引时，最左边的列一定要使用到，否则索引无效。

4. 【强制】超过三个表禁止join。

   需要join的字段，类型必须绝对一致；
   当多表关联查询时，保证关联的字段需要有索引；



**【推荐】**

1. 【推荐】利用覆盖索引进行查询，避免回表。

   覆盖索引指的是如果一个索引包含了所有需要查询的列，那么没必要再回到表里查询数据行了。
   覆盖索引是非常有用的工具，可以极大地提高性能。



### 索引选择策略

​	在网上很多博客都会说，尽量避免使用非等值判断，如 != ，IS NOT NULL， NOT IN 等，因为这样会使索引查询无效。那么为什么非等值判断就会使索引查询无效呢？非等值判断一定会使索引查询无效吗？这个问题很值得讨论和思考，因为它会让你弄懂MySQL索引真正的选择策略。

#### IS NOT NULL 不使用索引吗

​	如果我们对列值进行NULL 值判断，那么通常该列没有设置非空约束。我们分为两种情况讨论，有非空约束和没有非空约束的情况。下面是我多次测试的结果。

情况一（索引列没有非空约束）：

**非覆盖索引查询**

​	非空数据占比低于约10%的时候，判断 is not null 会使用索引，而 is null 会全表扫描；

​	非空数据占比高于约20%的时候，判断 is null 会使用索引, 而 is not null 则是全表扫描；

​	非空数据占比在10%-20%之间的时候, is null 和 is not null 都会使用索引；

**覆盖索引查询**

​	无论非空数据占比是多少，判断 is null 和 is not null 都会使用索引；



情况二（索引列有非空约束）：

既然有非空约束，那么该列就不会有空值，非空数据占比是100%；

**非覆盖索引查询**

​	判断 is null 会直接返回空集，既不会查询索引，也不会查询表；（因为非空约束和 is null 是互斥的两种情况）

​	判断  is not null  会进行全表扫描，同样不会使用索引；

**覆盖索引查询**

​	判断 is null 会直接返回空集，既不会查询索引，也不会查询表；

​	判断 is not null  会进行全索引扫描；（ref : index）

结论:

​	通常情况下，设置索引的列往往都是有值的，而且非空数据的占比很大，所以几乎所有开发者说 is not null 不会使用索引，其实按照这种规范去开发没什么大问题，只不过原理还是需要知晓的， **is null 和 is not null 到底用不用索引，其实没有这么绝对！ mysql优化器会判断操作索引和操作表哪个代价小一些，然后再做出相应的查询策略。**

​	使用覆盖索引查询的情况下, is null 和 is not null 都会使用索引, 此时和非空数据的占比无关;


#### != 不使用索引吗

​	上面用到 **非空数据占比** 这个词，在判断 is null 时，NULL 值就是满足条件的数据；在判断 is not null 时，非 NULL 值就是满足条件的数据，满足条件的数据量除以表的总记录数就是**满足条件的数据占比**。



**等值查询：( = )**
	满足条件的数据占比低于约90%时，使用索引查询；

​	如果满足条件的数据超过90%，那么使用索引后再回表查询，效率反而不高，所以这种情况MySQL不使用索引，直接全表扫描；

**不等于查询：( != )**
	满足条件的数据占比低于约20%时，使用索引查询；如果满足条件的数据超过20%， 则不会使用索引；

​	而当不满足等值条件查询的数据占比在10%-20%之间时,两种查询都会使用索引;

**结论:**

​	可以看出，MySQL这么设计符合二八定律原则。通常情况下， 等值查询返回的结果集占比不会超过80%，所以通常等值查询会使索引生效；
​	而当我们写不等于查询时， 如果查询返回的结果集占比超过20%，那么不等于查询会使索引查询无效；

​	经过分析和测试，我们知道，无论是等值查询还是不等于查询， 都不能单纯的说它走索引还是不走索引， mysql 根据是否为等值查询和满足条件的数据占比来选择合适的查询策略。



#### 覆盖索引查询一定生效吗

1. 限制索引长度的情况，覆盖索引无效;
2. 索引列参与表达式运算，不仅覆盖索引无效，索引查询也会无效;



## 6.5 索引方式（数据结构）

### B-Tree 索引

​	B-Tree 索引的列值是按顺序存储的，并且每一个叶子到根的距离相同。

#### 磁盘IO与预读

1. 磁盘IO的代价是很高的, 一次磁盘io （机械硬盘）的耗时会在 5-10 ms 之间。 
2. 根据局部性原理, 磁盘不仅读取目标数据,也会顺带把相邻数据一并读进内存. 每一次IO读取的数据我们称之为一页(page)。具体一页有多大数据跟操作系统有关，一般为4k或8k，也就是我们读取一页内的数据时候，实际上才发生了一次IO，这个理论对于索引的数据结构设计非常有帮助。

#### 索引的数据结构(B+树)

​	什么样的数据结构才能支持数据库索引这样的特性，其实很简单，那就是：**每次查找数据时把磁盘IO次数控制在一个很小的数量级，最好是常数数量级**。那么我们就想到如果一个高度可控的多路搜索树是否能满足需求呢？就这样，b+树应运而生。

​	注：在数据结构中B-树和B+树是不同的数据结构，但在数据库索引这里提的B-树就是数据结构中的B+树。

**B+树**



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200211220433208.png" alt="image-20200211220433208" style="zoom:80%;" />

​	如上图，是一棵B+树，关于b+树的定义可以参见相关资料，这里只说一些重点，浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色所示），如磁盘块1包含数据项17和35，包含指针P1、P2、P3，P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块。真实的数据存在于叶子节点即3、5、9、10、13、15、28、29、36、60、75、79、90、99。非叶子节点不存储真实的数据，只存储指引搜索方向的数据项，如17、35并不真实存在于数据表中。

**B+树的查找过程**

​	如图所示，如果要查找数据项29，那么首先会把磁盘块1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为非常短，相比磁盘的IO，可以忽略不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3由磁盘加载到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO。真实的情况是，**3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的**，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次的IO，显然成本非常非常高。

**B+树性质**

​	1. 通过上面的分析，我们知道IO次数取决于b+数的高度h，假设当前数据表的数据为N，每个磁盘块的数据项的数量是m，则有h=㏒(m+1)N，当数据量N一定的情况下，m越大，h越小；而m = 磁盘块的大小 / 数据项的大小，磁盘块的大小也就是一个数据页的大小，是固定的，如果数据项占的空间越小，数据项的数量越多，树的高度越低。这就是为什么每个数据项，即索引字段要尽量的小，比如int占4字节，要比bigint8字节少一半。这也是为什么**b+树要求把真实的数据放到叶子节点而不是内层节点**，一旦放到内层节点，磁盘块的数据项会大幅度下降，导致树增高。当数据项等于1时将会退化成线性表。

​	2. 当b+树的数据项是复合的数据结构，比如(name,age,sex)的时候，b+数是按照从左到右的顺序来建立搜索树的，比如当(张三,20,F)这样的数据来检索的时候，b+树会优先比较name来确定下一步的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当(20,F)这样的没有name的数据来的时候，b+树就不知道下一步该查哪个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。比如当(张三,F)这样的数据来检索时，b+树可以用name来指定搜索方向，但下一个字段age的缺失，所以只能把名字等于张三的数据都找到，然后再匹配性别是F的数据了， 这个是非常重要的性质，即**索引的最左匹配特性**。



#### 聚簇索引

- **聚簇索引并不是一种单独的索引类型，而是一种数据存储方式；**
- **一个InnoDB表有且只有一个聚簇索引；**
- **MySQL中的聚簇索引不能手动指定；**

​	InnoDB 使用的就是聚簇索引，聚簇索引实际上是在一个数据结构（B+树）中同时保存了索引项和数据行。术语“聚簇”表示的就是数据行和相邻的键值紧凑的存储在一起，即表数据和索引数据存储在同一个文件中。

​	因为无法把数据行同时存放在两个不同的地方，所以一个表只能有一个聚簇索引。



聚簇索引的选择：

1. 默认情况下，InnoDB 通过主键聚集数据，即主键索引就是聚簇索引；
2. 如果没有定义主键，InnoDB 会选择一个非空唯一的索引作为聚簇索引；
3. 如果上面两条都不满足，InnoDB 会隐式定义一个主键来作为聚簇索引；

怎么聚集数据？

1. 非叶子节点只包含部分用于检索数据的索引项；
2. 叶子节点包含了全部索引项，每个索引项指向一条行数据；



B-Tree 索引分为两种：

1. 聚簇索引
2. 辅助索引（二级索引）



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200620141347370.png?raw=true" alt="image-20200317082644072" style="zoom: 80%;" />



​	为什么辅助索引又叫二级索引呢？**因为通过辅助索引访问表数据需要两次索引查找，而不是一次。二级索引叶子节点保存的不是指向行的物理位置指针，而是行的主键值。**

​	这意味着通过二级索引查找行数据，存储引擎需要找到二级索引的叶子节点获得对应的主键值，然后根据主键值去聚簇索引中查找对应的行。这里是两次B-Tree查找而不是一次！不过InnoDB的自适应哈希索引可以减少这样的重复工作。



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200317082644072.png" alt="image-20200317082644072" style="zoom: 80%;" />



​	那么问题来了，为什么InnoDB在主键索引树的叶子节点存储了具体数据，而在二级索引树上却不存储具体数据，而是非要多此一举找到主键，再在主键索引树找到对应的数据呢？

​	其实很简单，因为InnoDB需要**节省存储空间**。

​	一个表里可能有很多个索引，InnoDB都会为每个加了索引的字段生成索引树，如果每个字段的索引树都存储了具体的数据，那么这个表的索引数据文件就会非常巨大（数据极度冗余了）。所以从节约磁盘空间的角度来说，真的没有必要每个字段索引树都存具体数据，在牺牲较少的查询性能下节省了巨大的磁盘空间，这是非常值得的。



InnoDB 创建表后生成的文件有：

- frm: 存储表结构数据 ;
- idb: 表数据 + 索引数据 ;



**总结**

​	**InnoDB表是基于聚簇索引建立的，聚簇索引对主键查询有很高的性能。二级索引的叶子节点包含主键列值（二级索引的叶子节点保存了对应行的主键值），所以如果主键列很大的话，其他的所有索引都会很大。因此，主键应当尽可能的小。**



### 哈希索引

​	B-Tree索引是按照数据大小顺序进行存储的，所以很适合范围查找。

​	而哈希索引不是按顺序存储的，哈希索引虽然可以做到快速检索数据，但是没有办法做到高效的范围查找，所以哈希索引不适合作为MySQL索引底层的数据结构。

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200317081955987.png?raw=true" alt="image-20200317082644072" style="zoom: 33%;" />



## 6.6 小结

​	从数据冗余存储的方面分析，一个应用系统的**缓存和索引**的设计都属于数据的冗余存储，主要目的都是为了提高系统的响应速度。

​	把磁盘中的数据读取到缓存中存储一份，是因为内存的读写速度远大于磁盘，所以缓存中的这部分冗余数据就加快了系统的响应速度。而把经常查询的数据设计为索引，索引文件中冗余这部分数据是按照特定的数据结构进行组织并存储的，因为高效的数据结构，所以读取速度非常快，从而加快系统响应速度。

​	**同样是冗余存储，缓存利用高效的硬件条件，索引利用高效的数据结构。**但无论是引入缓存还是索引，提升效率的同时，也会带来相应的问题，如引入缓存带来的数据一致性问题、缓存击穿、缓存雪崩、缓存穿透，引入索引会占用更多的磁盘空间，降低数据插入速度等，这些都是工程师需要考虑和解决的问题。



# 7. 事务（Transaction）UGtdySjuV



## 7.1 四个基本要素

数据库事务的四个基本要素：**ACID**

- 原子性（Atomicity）

- 一致性（Consistency）

- 隔离性（Isolation）

- 持久性（Durability）



一个支持事务（Transaction）的系统，具有这四种特性:

1、**原子性**（Atomicity）：

​	事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。

2、**一致性**（Consistency）：

​	事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。

3、**隔离性**（Isolation）：

​	同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。

4、**持久性**（Durability）：

​	事务完成后，事务对数据库的所有更改会持久化到到磁盘。



## 7.2 操作事务

操作事务的语句有3条：开启事务，提交事务，回滚事务。

### 开启 | 提交 | 回滚

**开启事务：** 

```mysql
begin;
# 同上
start transaction;
```

**提交事务：** 

```mysql
commit;
```

**回滚事务：** 

```mysql
rollback;
```



mysql默认是自动提交事务的（auto commit），查看 autocommit 变量

```mysql
# 1代表true，0代表false;
select @@global.autocommit;		
```

设置是否自动提交事务： 

```mysql
# 不自动提交事务
SET (GLOBAL) autocommit = 0;
# 自动提交事务
SET (GLOBAL) autocommit = 1;		
```

**注意：**

​	避免一个事务的时间过长，这样会降低数据库的并发性能，也会使锁等待超时、死锁出现的几率增大。

### 查看事务

在 information_schema 数据库中查看正在执行的事务：

```mysql
# 查看正在执行的事务
SELECT * FROM information_schema.INNODB_TRX;
```

输出：

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200617161354085.png" alt="image-20200617161354085" style="zoom: 200%;" />

​	事务的基本信息在 INNODB_TRX 表都可以查到，包括事务ID，事务状态，开始时间，持有的锁ID，事务操作的线程ID，事务隔离级别，是否正在操作表等。

### 强行结束事务

​	根据事务的线程ID（trx_mysql_thread_id），可以通过mysql命令 `kill`  杀掉线程，从而强行结束事务。

```mysql
mysql> kill 64;
Query OK, 0 rows affected (0.00 sec)
```



## 7.3 事务隔离级别

​	在SQL标准中定义了四种隔离级别，Mysql实现了这四个标准的事务隔离级别。

​	Mysql 默认的事务隔离级别是 `可重复读`。

| 事务隔离级别/并发问题        | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| 读未提交（read-uncommitted） | 是   | 是         | 是   |
| 读已提交（read-committed）   | 否   | 是         | 是   |
| 可重复读（repeatable-read）  | 否   | 否         | 是   |
| 串行化（serializable）       | 否   | 否         | 否   |

查看事务隔离级别

```mysql
# 查看mysql系统级别的事务隔离级别
SELECT @@global.tx_isolation;
# 查看mysql会话级别的事务隔离级别
SELECT @@session.tx_isolation;
SELECT @@tx_isolation;
```

设置事务隔离级别（新的隔离级别会在下一个事务开始的时候生效）

```mysql
-- 设置系统的事务隔离级别
set global transaction isolation level read committed;
-- 设置当前会话的事务隔离级别
set session transaction isolation level read committed;
```

注：

​	**Mysql 默认的事务隔离级别是 可重复读。（通过间隙锁策略防止幻读）**

​	Oracle 默认的事务隔离级别是 读已提交。



## 7.4 事务的并发问题

事务并发执行会造成相应的并发问题，根据不同的隔离级别，造成的并发问题也不一样：

### 脏读

​	如果事务隔离级别设置为**读未提交** `read-uncommitted`，这时事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据。

### 不可重复读

​	如果事务隔离级别设置为**读已提交** `read-committed`，事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果不一致。

### 幻读

​	系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。

注：

​	不可重复读的和幻读容易混淆，不可重复读侧重于**修改**，幻读侧重于**新增和删除**。



## 7.5 事务日志

​	事务日志可以提高事务的效率。使用事务日志，存储引擎在修改表的数据时，只需要修改其内存拷贝（log buffer），再把修改记录持久化到磁盘上的事务日志中（log file），而不用每次都将修改立即持久化到磁盘。

​	事务日志采用的是追加的方式，因此写日志的操作是顺序I/O，而不是随机I/O，采用事务日志的方式相对更快一些。事务日志持久化后，内存中被修改的数据在后台异步刷新到磁盘上，修改数据需要写两次磁盘。



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200219161013965.png" alt="image-20200219161013965" style="zoom:80%;" />



根据功能不同，事务日志分为：

-  `redo log` 
-  `undo log` 



### redo log

​	MySQL为了提升性能不会把每次的数据修改都实时的同步到磁盘，而是会先存到 log buffer 中，并把这个当做缓冲使用，后台线程再去做缓冲和磁盘之间的同步。

​	那么问题来了，如果来没来得及同步就异常宕机或断电怎么办？由此引入 redo log 文件来记录已成功提交事务的修改信息，并会把redo log 文件刷新到磁盘，系统重启之后读取 redo log 恢复最新数据。

​	**所以修改数据需要写两次磁盘：事务日志文件和表数据文件。**

​	redo log 记录的是数据的物理变化（在数据页上做了哪些修改）。

redo log 作用：

- 提高数据写入效率，顺序I/O效率优于随机I/O；
- 保障已提交事务的数据一致性和持久化特性；



**配置** 

事务日志相关的重要配置如下:

```shell
innodb_log_buffer_size			# 配置log_buffer大小
innodb_log_file_size			# 配置log_file单个文件大小
innodb_log_files_in_group		# 配置log_file组员个数
innodb_flush_log_at_trx_commit	# 配置事务日志刷新策略
innodb_flush_log_at_timeout		# 配置事务日志刷新频率
```

**innodb_log_buffer_size** 

​	日志缓冲大小，mysql5.6 默认 8M大小；一般默认即可，具有大量事务操作的系统可以适当增大这个值；

```shell
mysql> show variables like '%innodb_log_buff%';
+------------------------+---------+
| Variable_name          | Value   |
+------------------------+---------+
| innodb_log_buffer_size | 8388608 |
+------------------------+---------+
```



**innodb_log_file_size**

​	InnoDB 存储引擎使用一个指定大小的  **redo log 存储空间**， redo log 空间 由 `innodb_log_file_size` 和 `innodb_log_files_in_group` 两个参数来调节。两个参数相乘的总和就是 redo log 的存储空间（单位：字节）。

​	如上图，正好和 data目录下的 ib_logfile0 和 ib_logfile1 相对应，每个文件大小为 50331648字节（约50M）。

```shell
mysql> show variables like '%innodb_log_file%';
+---------------------------+----------+
| Variable_name             | Value    |
+---------------------------+----------+
| innodb_log_file_size      | 50331648 |
| innodb_log_files_in_group | 2        |
+---------------------------+----------+
```

​	理论上两个参数都可以调节 redo log 空间，但我们一般只通过 `innodb_log_file_size`  参数来调节。为InnoDB 存储引擎 配置合适的redo log 空间对于写敏感的工作来说是非常重要的，你配置的 redo log 空间越大，InnoDB 就能更好的优化写操作，然而这项工作也是需要权衡利弊的，弊端就是  redo log 空间越大，mysql在崩溃（断电、意外宕机）后恢复的时间越长。

​	如果恢复时间对你的生产环境影响很大，那么修改该参数更需要慎重，建议模拟系统崩溃来测试修改参数后的影响。

:warning: 如果SQL插入的数据 大于redo log 空间的 10%，则会报错：

```shell
[ERR] The size of BLOB/TEXT data inserted in one transaction is greater than 10% of redo log size. Increase the redo log size using innodb_log_file_size.
```

​	如果redo  log 的空间是100M，那么在一次事务中插入的数据超过10M，就会插入失败；可通过设置 innodb_log_file_size 参数调整 redo log 的大小，mysql5.6默认大小为 50,331,648 字节，约50M。



data 目录下的 ib_logfile0 和 ib_logfile1 就是 redo log 文件:

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200514213816915.png" alt="image-20200514213816915" style="zoom:80%;" />

**innodb_flush_log_at_trx_commit**

​	**此参数用于控制事务提交的策略，即控制 redo log 刷新到磁盘的方式。该参数对mysql服务的影响非常大！这个参数有3个值：（0、1、2），默认为1。三个值的策略如下：**

- 1：每次提交事务会**立即**将 log_buffer 中的数据写入到  log_file 中，同时也会**立即**触发文件系统到磁盘的同步；（默认为1）
- 0： 每次提交事务不会立即把缓冲数据写入到日志文件，log_buffer 中的数据将会以**每秒一次的频率**写入到  log_file 中，同时会触发文件系统到磁盘的同步；

- 2：每次提交事务会立即将 log_buffer 中的数据写入到  log_file 中，但不会立即触发文件系统到磁盘的同步，同步频率为一秒一次；



```shell
mysql> show variables like '%innodb_flush_log%';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_flush_log_at_timeout    | 1     |
| innodb_flush_log_at_trx_commit | 1     |
+--------------------------------+-------+
```

分析：

​	默认为策略1，该策略最安全，性能相对其他两个来说也最低。

​	策略2的性能略高，安全性略低。在文件系统损坏或者突然宕机的情况下，会丢失最后一秒的所有事务。

​	值为0的策略性能最高，安全性也是最低的。mysql宕机有可能丢失最后一秒的所有事务，且丢失的数据理论上比策略2还要多。

​	高性能和高安全就像鱼和熊掌，不可兼得。需要根据实际场景去做选择。



**innodb_flush_log_at_timeout**

​	该参数用于指定InnoDB 事务日志刷新的时间频率，默认为1秒。注意，该参数和 commit 动作无关。



### undo log

​	**`undo log`主要有两个作用：支持回滚和多版本并发控制（MVCC）。**

​	undo log 用于记录数据被事务修改之前版本的信息，假如由于系统错误或者 rollback 操作而回滚的话，可以根据 undo log 信息回滚到没被修改前的状态。

​	假如由于系统错误或者 rollback 操作而回滚的话，可以根据 undo log 信息回滚到没被修改前的状态。

​	undo log 的记录方式不同于redo log，undo log 记录的是数据的**逻辑变化**，它会记录数据修改的反向操作。

​	undo log 默认存放在共享表空间中，即 ibdata1 文件。可以通过修改系统变量将undo log在ibdata1 文件中剥离出来:

```properties
innodb_undo_directory = .
# undo log 文件的个数,默认为0,代表不开启独立undo表空间,undo日志记录在在ibdata文件中
innodb_undo_tablespaces = 0		
```

注：

​	**undo log 为回滚数据提供了保障，保障未提交事务的原子性。**

​	**每条数据变更操作（增删改）都伴随着一条 undo log 的生成，并且回滚日志必须先于数据持久到磁盘上。**



日志文件的持久化和数据持久化都涉及到磁盘IO，有什么区别？

1. 事务日志文件的存储是顺序存储，顺序IO比随机IO更快。
2. 缓存同步是以数据页为单位的，每次同步的数据大小大于 单次操作的数据量，提高了性能。

​	

注：

​	以上配置参数，你可能不经常甚至从来都没有修改过，这很正常，但是知道这些系统配置参数对于你理解MySQL的运行机制有非常大的帮助。



# 8. 数据库锁（Lock）UGtdySjuV

​	数据库的锁机制是由存储引擎提供的，而每个存储引擎的实现策略不相同，这里重点对比下MyIsAM 和 InnoDB 存储引擎。

| 对比项 | InnoDB     | MyIsAM |
| ------ | ---------- | ------ |
| 事务   | 支持       | 不支持 |
| 锁     | 行锁、表锁 | 表锁   |
| 外键   | 支持       | 不支持 |



## 8.1 隐式和显式锁定

​	InnoDB采用两阶段锁定协议：隐式锁和显式锁。

**隐式锁：**

​	在事务执行过程中，随时都可以执行锁定，锁只有在事务提交commit 或回滚 rollback 时才会释放，并且所有的锁是在同一时刻被释放。

​	这就是隐式锁，InnoDB 会根据隔离级别在需要的时候自动加锁。

**显式锁：**

1. 同样，InnoDB 也支持通过特定语句进行显式锁定。（这些语句不属于SQL规范）

```mysql
SELECT ... LOCK IN SHARE MODE;		-- 显式上共享锁
SELECT ... FOR UPDATE;				-- 显式上排他锁
```

2. MySQL也支持 `LOCK TABLES` 和 `UNLOCK TABLES` 语句，这是在服务层实现的，和存储引擎无关。它们有自己的用途，但并不能代替事务处理。

```mysql
LOCK TABLES student READ;		-- 显式给整张表上共享锁
LOCK TABLES student WRITE;		-- 显式给整张表上排他锁

UNLOCK TABLES;					-- 释放当前会话持有的所有锁
```

注：

​	`LOCK TABLES` 和 `UNLOCK TABLES`  要在同一个会话中成对使用，在当前会话中加锁，去另一个会话解锁是无效的；`UNLOCK TABLES`  是释放当前会话持有的所有锁。

​	`LOCK TABLES` 和 `UNLOCK TABLES` 常在数据库备份场景下使用，保证数据备份时的一致性。

​	`LOCK TABLES` 和事务锁之间互相影响的话，情况会变复杂，而且影响性能，所以一般情况下没必要使用该语句。

​	上面显式的通过SQL上共享锁和排他锁，锁住的资源是整张表还是某几行，得视情况分析。

**特别注意:**

​	**InnoDB 存储引擎提供的显式上锁，无论是共享锁还是排他锁，其他事务都可以进行普通的select 查询**，因为普通的select 查询不需要加锁，不会出现锁冲突；而使用 `LOCK TABLES ... WRITE` 提供的排他表锁，其他事务是无法对该表进行任何操作的，即便是普通的select 查询也不可以，这一点是比较大的区别。



在 information_schema 数据库中查看事务的锁信息：

```mysql
# 查看正在执行的事务
SELECT * FROM information_schema.INNODB_TRX;
# 查看正在持有锁的事务
SELECT * FROM information_schema.INNODB_LOCKS;
# 查看正在等待锁的事务
SELECT * FROM information_schema.INNODB_LOCK_WAITS;
```



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200617160405850.png" alt="image-20200617160405850" style="zoom:80%;" />



```mysql
mysql> select * from information_schema.INNODB_LOCK_WAITS;
+-------------------+---------------------+-----------------+---------------------+
| requesting_trx_id | requested_lock_id   | blocking_trx_id | blocking_lock_id    |
+-------------------+---------------------+-----------------+---------------------+
| 1152757           | 1152757:25869:3:113 | 1152752         | 1152752:25869:3:113 |
+-------------------+---------------------+-----------------+---------------------+
```



## 8.2 读写锁

在 Mysql 中实现了两种基本的锁：

**共享锁**：

​	又叫读锁（read lock），其他事务可以继续加共享锁，但是不能继续加排他锁。
​	
**排他锁**：

​	又叫写锁（write lock），一旦加了写锁之后，其他事务就不能加任何锁了。



读锁和写锁的兼容关系:

|      | 读锁 | 写锁 |
| ---- | ---- | ---- |
| 读锁 | 兼容 | 冲突 |
| 写锁 | 冲突 | 冲突 |

​	锁兼容是指事务 A 获得锁之后，事务 B 也尝试获取某种锁，如果能立即获取，则称锁兼容，反之叫冲突。

​	纵轴是代表已有的锁，横轴是代表尝试获取的锁。

## 8.3 锁力度

​	加锁一定是有开销的，一般来说，加锁的粒度越小，并发度越高，开销也就越大。MySQL的InnoDB 存储引擎提供了两种锁策略：表锁和行锁。

### 表锁

​	**表锁是Mysql中最基本的锁策略，是开销最小的锁策略：它会直接锁住整张表的数据。**

场景:

​	当对一个表进行当前读操作（insert, delete, update, select...for update），并且没有使用索引时，事务操作期间就会锁住整张表，这会阻塞其他事务对该表的所有当前读操作，表锁的并发效率低。

注：写操作一定是当前读操作，当前读并不一定是写操作，比如 select...for update 就是当前读，但不是写操作。

### 行锁

​	**行级锁可以最大程度的支持并发处理，同时也带来了较大的锁开销。** 

​	**行锁是建立在使用索引的基础之上的。**事务操作只有使用了索引，才有可能使用行锁。



## 8.4 MVCC

​	如果每次读或写都需要加相应的锁来保证数据的安全性，那么并发性能其实是不高的，InnoDB 通过MVCC 提高并发性能（这是重头戏）。

MVCC（Multi-Version Concurrency Control）多版本并发控制：

​	**在InnoDB 表中，每一行记录的后面增加两个隐藏列，创建版本号和删除版本号。**

​	每开始一个新的事务，系统版本号都会递增，事务开始时刻的系统版本号会作为事务的版本号。对于查询，事务版本号会和每一行的两个隐藏列作对比；对于增删改，事务版本号都会新增到相应的隐藏列中。



下面看看在 `可重复读` 隔离级别下，MVCC具体是如何操作的：

**SELECT**

InnoDB会根据以下两个条件检查每行记录：

1. InnoDB只查找创建版本号 **小于等于** 当前事务版本号的数据行。

   这样可以确保事务读取到的行，要么是在事务开始之前就存在的，要么是事务自身插入或修改过的。

2. 行的删除版本要么未定义，要么 **大于** 当前事务版本号。

   这样可以确保事务读取到的行，要么没被删除，要么是在当前事务开启之后被删除的。

只有符合上述两个条件的记录，才能返回作为查询结果。



**INSERT**

​	InnoDB 为新插入的每一行保存当前系统版本号（事务版本号）作为创建版本号。

**DELETE**

​	InnoDB 为删除的每一行保存当前系统版本号（事务版本号）作为删除版本号。

**UPDATE**

​	InnoDB 插入一行新纪录，保存当前系统版本号（事务版本号）作为创建版本号；
​	同时保存当前系统版本号（事务版本号）作为原来行的删除版本号。



​	保存这两个额外的系统版本号，使绝大多数的读操作都可以不用加共享锁。这样的设计使得读操作很简单，性能也很好，并且能够保证只会读取到符合标准的行。

​	不足之处是每行记录都需要额外的存储空间，需要做更多的行检查工作，以及一些额外的维护工作。



在 MVCC 中，分为两种读操作：

**当前读：**

​	需要加锁的语句，update，insert，delete，select...for update 等都是当前读。

**快照读：**

​	MVCC 实现了可重复读，**在 `可重复读` 隔离级别下的快照读，事务不是以 `begin` 开始的时间点作为快照建立时间点，而是以第一条 select 语句的时间点作为快照的时间点，以后的 select 都会读取当前时间点的快照值，已达到可重复读的目的。**



MVCC最大的好处：**读不加锁，读写不冲突。读写不冲突极大的增加了系统的并发性能。**

**小结：**

​	**MVCC 只在 读已提交 和 可重复读 两个隔离级别下工作。**其他两个隔离级别都和MVCC不兼容，因为读未提交总是读取最新的数据行，而不是符合当前事务版本的数据行；而串行化则会对所有读取的行都加锁。



## 8.5 间隙锁

​	MySQL默认的事务隔离级别是 `可重复读` ，可重复读存在**幻读**的并发问题，MySQL使用**间隙锁**来防止幻读。

下面用例子来一睹间隙锁的风采：

（student_sign：学生签到表；id为主键，其他字段都没有索引）

```sql
mysql> select * from student_sign;
+----+---------+------+-----+---------+
| id | version | name | age | is_sign |
+----+---------+------+-----+---------+
|  1 | 1       | wyc  |  25 | 1       |
|  2 | 2       | ybw  |  45 | 0       |
|  3 | 3       | jjj  |  42 | 1       |
|  4 | 3       | www  |  56 | 1       |
|  5 | 3       | www  |  22 | 1       |
|  7 | 7       | xxx  |  33 | 1       |
|  8 | 4       | wws  |  77 | 0       |
|  9 | 9       | rrr  |  56 | 1       |
| 10 | 7       | ttt  |  65 | 1       |
+----+---------+------+-----+---------+
```

### 场景一

事务A：

```sql
mysql> begin;
mysql> update student_sign set is_sign = 1 where id >=4 and id <=8;
Query OK, 4 rows affected (0.00 sec)
```

事务A在没有提交的情况下，开启事务B：（模拟并发）

```sql
mysql> begin;
mysql> insert into student_sign (id,version,name,age) VALUES (6,3,'www',43);			
-- 会一直阻塞该SQL的执行，直到锁等待超时；

mysql> update student_sign set age =88 where id =3;			
-- 执行成功，不会阻塞；

mysql> update student_sign set age =88 where id =9;			
-- 会一直阻塞该SQL的执行，直到锁等待超时；

mysql> update student_sign set age =88 where id =10;			
-- 执行成功，不会阻塞；
```

结论：

​	where条件对**索引字段**进行**范围写操作**，范围[m,n],   间隙锁会锁定 [m, n+1] 范围内的连续数据(存在和不存在的)。

​	所以第1、3条SQL语句会一直阻塞，因为它们操作的数据在间隙锁范围之内。倘若第一条SQL执行成功，那么就发生了幻读。

​	第2、4条SQL语句可以执行成功，它们操作的数据不在间隙锁范围之内。在解决幻读的情况下，提高了并发效率。



### 场景二

事务A：

```sql
mysql> begin;

mysql> update student_sign set is_sign = 1;		-- 没有where条件，进行全表更新
```

事务A在没有提交的情况下，开启事务B：

```sql
mysql> begin;

mysql> insert into student_sign (id,version,name,age) VALUES (16,3,'www',43);			
-- 会一直阻塞该SQL的执行，直到锁等待超时；
1205 - Lock wait timeout exceeded; try restarting transaction
```

结论：

​	不带where条件的update 和 delete 语句，事务期间会锁整张表。其他事务对该表的所有写操作，都会被阻塞。

### 场景三

事务A：

```sql
mysql> begin;
mysql> update student_sign
set is_sign = 1
where age>18 and age < 60 ;			
Query OK, 1 row affected (0.00 sec)
Rows matched: 7  Changed: 1  Warnings: 0

mysql> 
```

事务A在没有提交的情况下，开启事务B：

```sql
mysql> begin;
mysql> insert into student_sign
(id,version,name,age)
VALUES
(16,3,'www',43);			-- 会一直阻塞该SQL的执行，直到锁等待超时；
1205 - Lock wait timeout exceeded; try restarting transaction
```

结论：

​	where条件对**非索引字段**进行**范围写操作**，事务期间会锁整张表。其他事务对该表的所有写操作，都会被阻塞。



## 8.6 乐观锁

乐观锁是一种思想，是一个策略，并不是一个真实存在的锁，乐观锁的概念是相对于悲观锁来说的。

MySQL的乐观锁可通过版本号实现，即每次更新数据通过**主键和版本号**联合更新：

```sql
update student set name = "Daming" 
where
id = 2 and version = 10200;
```

版本号的维护:

​	每次新增数据，初始化一个版本号；每次修改数据，一并修改版本号（通常是递增版本号）；版本号字段值不能重复，可以不设置索引。



## 8.7 死锁

死锁是指两个或多个事务争夺同一个资源，对方都持有自己想要的资源而又都不放锁。

### 产生死锁

比如下面两个事务同时处理：(id 为主键)

```mysql
-- 事务1
begin;
update students set name = 'jack' where id=3;
update students set name = 'brian' where id=4;
commit;

-- 事务2
begin;
update students set name = 'brian' where id=4;
update students set name = 'jack' where id=3;
commit;
```

​	在并发情况下，如果两个事务都同时执行完第一条update语句，持有了该行的排他行锁，接着每个事物都去尝试执行第二条update 语句，却发现该行已经被其他事务锁定，于是两个事务都等待对方释放锁，同时都持有对方需要的锁，于是陷入死循环。

​	上面这个例子是两个事务使用行锁时引发的死锁，使用表锁同样也可能产生死锁。除非有外部因素介入才可能解除死锁。



### 处理死锁

InnoDB处理死锁的方式很简单：将持有最少锁资源的事务进行自动回滚。

```mysql
1213 - Deadlock found when trying to get lock; try restarting transaction
```

在MySQL5.6 及更低版本中，死锁检测是自动开启的，并且无法关闭。

**关闭死锁检测**

​	在MySQL5.7 及更高版本中，可以通过 `innodb_deadlock_detect` 参数关闭死锁检测（默认开启），从而进一步提高并发性能。对于高并发的系统，当大量线程等待同一把锁时，死锁检测可能会导致性能下降，此时，禁用死锁检测，改为依靠 `innodb_lock_wait_timeout` 的锁等待超时（默认50秒）引发事务回滚会更加高效。

​	在数据库压力不大的情况下，不应该关闭死锁检测；只有当确定死锁检测确实影响了数据库的并发性能，才应该考虑关闭死锁检测。



### 查看死锁日志

```mysql
show engine innodb status;
```

在打印出来的信息中找到 “LATEST DETECTED DEADLOCK” 内容，就可以分析产生死锁的原因了。







### 死锁避免

​	控制加锁的先后顺序，是防止死锁行之有效的方法。



## 8.8 参数

锁相关的参数：

```properties
# 锁等待时间:秒
innodb_lock_wait_timeout = 50
```



# 9. 日志（Log）UGtdySjuV

日志是一个应用服务的重要组成部分。

mysql数据库服务的常用日志有：

​	错误日志		（默认开启）

​	慢查询日志	（默认关闭）

​	常规日志		（默认关闭）

​	二进制日志	（默认关闭）



mysql的日志开启和关闭方式，分为两种：

1. 在配置文件添加选项参数（需要重启服务器，且永久生效）;
2. 命令行直接修改（不需要重启服务器，重启失效）;



## 9.1 错误日志

错误日志默认开启，并且无法关闭。

通过命令查看错误日志的存放目录：

```sql
mysql> show variables like '%error%';
+---------------+-----------------------------------------+
| Variable_name | Value                                   |
+---------------+-----------------------------------------+
| log_error     | D:\mysql-5.6.46-winx64\data\Jackpot.err |
+---------------+-----------------------------------------+
1 row in set (0.04 sec)
```



## 9.2 慢查询日志

​	慢查询日志是用来记录查询时长超过指定时间的日志。

​	慢查询日志主要用来记录查询时间较长的语句，通过慢查询日志可以找出执行时间较长、执行效率低的语句，然后进行优化。

### 启用 | 关闭

Mysql的慢查询日志是**默认关闭**的，可以通过配置文件的 `log-slow-queries` 选项打开。

在启用慢查询功能时，同时也需要指定一个记录阀值，如果查询语句超过了这个值，Mysql将会记录慢查询日志。

```ini
[mysqld]
log-slow-queries[ =path/file_name ]
long_query_time = 10;		-- 单位秒
```

如果不指定目录和文件名称，默认存储在 ./data/hostname-slow.log 里（hostname是本主机名）。

如果不设置记录阀值，默认时间是10秒。



关于慢查询日志的相关命令如下：

```sql
-- 查看是否开启慢查询日志
show global variables like 'slow_query_log%';
-- 开启慢查询日志
set global slow_query_log = on;	
-- 关闭慢查询日志
set global slow_query_log = off;

-- 查看慢查询日志位置
SHOW VARIABLES LIKE 'slow_query_log_file';
-- 查看慢查询记录阀值(单位:秒)
SHOW VARIABLES LIKE 'long_query_time';
-- 设置慢SQL执行时间 只有新session才生效
SET GLOBAL long_query_time = 5;
```



### 查看慢查询日志

慢查询日志是文本文件，可以用记事本打开。

也可以用mysql 自带的 `mysqldumpslow` 工具进行汇总，排序，以便找出耗时最高，请求次数最多的慢查询日志。

除了 mysqldumpslow 工具外，还有很多优秀的第三方工具可以使用，例如 mysqlsla, myporfi 等。



### 删除慢查询日志

如果慢查询日志过大，需要删除日志以回收空间，可以通过以下命令将慢查询日志文件重置：



```sql
-- 删除慢查询日志文件
set global slow_query_log=0;		
-- 重新生成慢查询日志文件
set global slow_query_log=1;		
```



## 9.3 常规日志

​	常规日志记录了用户的所有操作，包括增删改查信息。在高并发环境下会产生大量信息，从而导致不必要的磁盘io，会影响Mysql的性能。

​	常规日志默认关闭。



### 启用 | 关闭

通过命令来开闭常规日志：

```mysql
# 开启
SET global general_log = on;	
# 关闭
SET global general_log = off;	
```



通过配置文件开启常规日志：

```ini
[mysqld]
log = path/file_name 
```

常规日志的默认存放在 ./data/hostname.log  中。



查看常规日志状态：

```mysql
mysql> show variables like 'general%';
+------------------+-----------------------------------------+
| Variable_name    | Value                                   |
+------------------+-----------------------------------------+
| general_log      | OFF                                     |
| general_log_file | D:\mysql-5.6.46-winx64\data\Jackpot.log |
+------------------+-----------------------------------------+
2 rows in set (0.05 sec)
```



## 9.4 二进制日志

​	二进制日志 binlog 记录了**所有用户**对数据库的**逻辑修改操作**。（creat ，drop，alter ，insert，delete，update，truncate 等操作，不会记录 select 操作）

​	binlog 实质上是MySQL在服务层实现的数据复制机制。

​	**binlog 主要有两个作用：复制和恢复。**

- MySQL在生产环境中大多是一主多从的结构，从服务器需要和主服务器的数据保持一致，就是通过binlog来复制的。
- 数据库的数据出现了勿删或部分丢失，可通过 binlog 来恢复数据。



**MySQL默认不开启二进制日志**，可以通过命令查看二进制日志的开启状态：

```mysql
mysql> show binary logs;
1381 - You are not using binary logging

# 通过变量查看二进制日志状态
mysql> show variables like 'log_bin%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin                         | OFF   |
| log_bin_basename                |       |
| log_bin_index                   |       |
| log_bin_trust_function_creators | OFF   |
| log_bin_use_v1_row_events       | OFF   |
+---------------------------------+-------+
5 rows in set (0.04 sec)
```

### 开启 Binlog

在mysql的配置文件加入如下配置，**重启服务后生效**：

```properties
# 开启binlog,并指定日志文件的存放位置和名称 
log-bin = /var/data/binlog_name
# 指定日志记录方式,有三种可选项: statement, row, mixed
binlog-format = row
```

注:

​	`log-bin` 属性的值用于指定binlog 的日志名称，文件名称自定义，而且是非必须的，如果空着不写，默认日志名称为 hostname-bin ，强烈推荐明确指定binlog 的日志名称，所有服务器应该都是一样的（主从架构），不要使用默认的主机名，如果主机名发生变化，日志名也会发生变化，此时会带来意想不到的麻烦。

再次查看binlog 开启状态：

```mysql
mysql> show binary logs;
+--------------------+-----------+
| Log_name           | File_size |
+--------------------+-----------+
| binlog_name.000001 |       120 |
+--------------------+-----------+
1 row in set (0.02 sec)
```

注: binlog_name.index 索引文件中存放的是所有有效 binlog 文件的位置和名称。



### 日志记录方式

`binlog-format` 属性的值用于指定binlog的日志记录方式。

MySQL binlog 有三种记录方式：

- statement 
- row 
- mixed 

statement 基于语句的记录方式：

​	每一条修改数据的SQL都会记录在binlog中。优点是不需要记录每一行的变化，减少binlog日志量，减少IO，提高性能。缺点引起的问题比较大，下面会以警告的方式着重讲解。

row 基于行的记录方式 ：

​	每一行记录的修改信息都会被记录在binlog中。比如一条update语句，修改了1000条数据，那么1000条数据的修改细节都要记录下来，很显然，这种方式比 Statement 记录的日志量要大的多，尤其是一条语句影响多行的情况下，每行记录的变化都会被记录到binlog中。

mixed 混合方式：

​	Mixed 是前面两种方式的混合使用。一般情况下会使用 statement 方式记录日志，当语句的运行结果不确定时，会以row 的方式记录日志，使用row 的场景大都是使用一些函数，具体哪些场景使用 row 方式可在官网文档查看。



**特别注意：** 

​	基于语句的方式记录日志可能会在主库和从库之间产生数据不一致的情况，主库与从库之间是通过binlog来复制数据的，如果基于语句方式记录日志，当使用函数更新数据，如 now() 时间函数，由于网络延时，在主库和从库上执行相同的SQL很容易产生不一样的数据，这便是 statement 带来的弊端。所以**不推荐使用 statement 方式记录日志，推荐使用 row 方式记录日志**。（阿里云数据库的binlog 也是 row 格式的）



### 常规配置

```mysql
# 开启binlog，并指定日志名称
log-bin = mysql-bin
# 指定binlog 记录格式,支持三种: statement, row, mixed
binlog-format = row

# 只记录指定数据库的binlog
binlog_do_db = db1
# 不记录指定数据库的binlog
binlog_ignore_db = db2

# binlog 日志缓冲区大小,默认32K,该参数基于会话,不应设置过大;
binlog_cache_size = 32768
# binlog 文件最大值,默认1G;
max_binlog_size = 1073741824

# binlog 刷新到磁盘的频率,0代表不控制,随着操作系统刷新; 1代表每提交1次事务就会触发一次刷新;
sync_binlog = 0
```

**sync_binlog** 

​	默认设置是 `sync_binlog = 0` ，即不强制控制磁盘进行刷新，由操作系统自行调度，这是性能是最好的，但风险也是最高的，一旦宕机，文件系统缓存里的 binlog 都会丢失。

​	设置 `sync_binlog = 1` ，表示每提交1次事务就会触发一次磁盘刷新，这种配置是最安全的，但性能却最差；可以配置为 0 或 100、1000等值，在安全性和性能之间取一个折中方案。

**binlog_cache_size**

​	日志缓冲区大小，默认为32K，该参数基于会话，不应设置过大；当事务的记录数据大于设定的值时，mysql会把缓冲区的数据写到一个临时文件中，所以该值也不能过小。

​	可以通过状态变量 `binlog_cache_use` 和 `binlog_cache_disk_use` 来测试 binlog_cache_size 设置的值是否合理：

```mysql
mysql> show global status like 'binlog_cache_%';
+-----------------------+--------+
| Variable_name         | Value  |
+-----------------------+--------+
| Binlog_cache_disk_use | 4712   |	#记录了使用临时文件写二进制日志的次数
| Binlog_cache_use      | 896035 |	#记录了使用缓冲的次数
+-----------------------+--------+
```

**注：如果使用临时文件的次数越来越多，那么应该适当增加日志缓冲区的大小，如调整为64K 或 128K大小。**



### 查看日志

```mysql
show binary logs;       # 查看binlog文件列表和大小;
show master logs;		# 查看master的binlog日志列表;
show master status;		# 查看master的最后一个binlog记录的相关信息;
flush logs;				# 新增一个binlog并往里面写日志;
```



查看二进制日志文件的列表：

```mysql
mysql> show binary logs;
+------------------+-----------+
| Log_name         | File_size |
+------------------+-----------+
| mysql-bin.000643 | 175493927 |
| mysql-bin.000644 |   8655055 |
| mysql-bin.000645 | 585464096 |
| mysql-bin.000646 | 133101420 |
+------------------+-----------+
4 rows in set (0.04 sec)
```

**查看二进制日志内容：**

​	binlog 以二进制的方式存取，不能直接查看，可以通过MySQL提供的 `mysqlbinlog` 工具查看。

```shell
shell> mysqlbinlog var/data/mysql-bin.000001
```



### 删除日志

binlog 可以手动删除，也可以自动删除。

**自动删除：** 

```mysql
# 设置日志保留30天，30天后自动删除；
set global expire_logs_days = 30;        
```

**手动删除：**

```mysql
# 删除日志索引中指定日期之前的binlog文件
purge master logs before '2018-03-30 18:00:00';
# 删除日志索引中指定的binlog文件
purge master logs to 'binlog.000002';       
# 删除 master的所有的binlog数据;
reset master;
# 删除 slave 的中继日志;
reset slave;       
```



# 10. 服务高可用 (high availability)UGtdySjuV

## 10.1 主从复制（Master-Slave）

​	MySQL 提供了主从复制功能来提高服务可用性，MySQL的主从复制是基于binlog（二进制日志）实现的，master 和 slave 之间通过二进制日志来同步数据。

### 原理

​	主服务器开启 `binlog` 记录数据库的所有修改操作，从服务器的IO线程使用专用账号登录到主服务器获取 binlog 数据，并将 binlog 写入到自己的中继日志 `relay log` 中，然后从服务器的SQL线程根据 relay log 执行SQL同步数据。



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200510105215845.png" alt="image-20200510105215845" style="zoom:80%;" />



### 作用

1. 可以作为**备份**机制，相当于热备份；
2. 可以用来实现**读写分离**，均衡数据库负载；



### 配置

**准备工作**

1、将主服务器要同步的数据库加锁，避免同步数据时数据发生变化：

```mysql
use db;
flush tables with read lock; 
```

2、将master 的数据导出：

```mysql
# 导出表结构和表数据,以及函数和存储过程;
mysqldump -u root -p -R db > db.sql;	
# 只导出表结构和表数据
mysqldump -u root -p db > db.sql;
# 只导出函数和存储过程;
mysqldump -u root -p -R -ntd  db > db.sql;		
```

3、将备份数据还原到从服务器：

```mysql
create database db;
use db;
source db.sql;
```

至此，master 和 slave 拥有同样的初始数据了。

4、备份完成，解锁数据库：

```mysql
unlock tables;
```



**主服务器配置**

1、修改配置文件：

```properties
# 开启binlog                
log-bin = mysql-bin
# 声明需要复制的数据库 (如果复制多个库重复设置即可)
binlog-do-db = db  
# salve 从 master上获取更新同步到自己的二进制文件中
log-slave-updates 
# 控制binlog 写入频率;默认为0,设置为1代表每提交一次事务就同步一次;
sync_binlog = 1  
#主数据库端ID号
server_id = 1  
# binlog 自动删除天数
expire_logs_days = 14  
#将函数复制到slave  
log_bin_trust_function_creators = 1 
```

2、重启mysql，创建salve 同步数据的账号：

```mysql
#创建slave账号 account，密码123456
grant replication slave on *.* to 'salve_account'@'10.10.20.116' identified by '123456';
#更新数据库权限
flush privileges;
```

3、查看master状态：

```mysql
show master status;
```



**从服务器配置** 

1、修改配置文件：

```properties
server_id = 2               
log-bin = mysql-bin 
replicate-do-db = db 
log-slave-updates 
sync_binlog = 0  
expire_logs_days = 15  
slave-net-timeout = 60
log_bin_trust_function_creators = 1 	#将函数复制到slave 
```

2、重启mysql，执行同步命令：

```mysql
#执行同步命令，设置主服务器ip，同步账号密码，同步位置
mysql>change master to master_host='10.10.20.111', master_port=3306,master_user='account',master_password='123456',master_log_file='mysql-bin.000003',master_log_pos=337523;

#开启同步功能
mysql>start slave;
```

3、查看 slave 状态：

```mysql
mysql>show slave status;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.10.20.111
                  Master_User: account
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000033
          Read_Master_Log_Pos: 337523
               Relay_Log_File: db-relay-bin.000002
                Relay_Log_Pos: 337686
        Relay_Master_Log_File: mysql-bin.000033
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
```

Slave_IO 线程和 Slave_SQL 线程必须正常运行，确保为 YES 状态，否则同步失败，失败信息可在错误日志中定位。



## 10.2 读写分离

​	只在主服务器上进行写操作，主服务器数据同步到从服务器上， 在从服务器上进行读操作。配置主从复制是实现读写分离的前提。



# 11. 备份与恢复UGtdySjuV

​	说数据库备份之前先讲一个发生在我身边的小故事，在我开始做程序员的时候，经常会在网上看到误删数据库的帖子，甚至同事们之间偶尔还拿这种话题开玩笑，当时我挺纳闷的，误删数据库到底是怎么做到的呢？这个疑问在我心中徘徊了很久，直到有一天，我身边的一个同事真的把生产环境的数据库给删了！

​	开始他找我求助时只是说数据库服务无法启动了，还把启动报错给我看，然后他又说他在网上看了个帖子，执行了命令 `rm -rf /var/lib/mysql` ，然后服务就再也起不来了，当时我一声 `wo cao` 就给他发过去了，确认后发现真的是把数据库的数据文件给删除了!这还不是最可怕的，最可怕的是生产环境的数据库居然没有开定期的数据备份，更没有开二进制日志备份。能找到的最近的一次逻辑备份还是几个月之前的数据。**在软件开发行业里，删除数据库算是一级重大事故了，删除数据库后发现还没有数据备份，那没有比这种情况更严重的事故了。**

​	所以删库还不是最可怕的事情，如果备份策略很完善，即便删库，也有“后悔药”可吃，不会造成很大的损失，但如果删了库还没有数据备份，那后果是糟糕到不能再糟糕了。

## 11.1 备份方法

​	数据备份的本质是建立冗余数据进行数据转储。根据备份过程中数据库服务的运行状态，可分为三类：

- 冷备份
- 热备份
- 温备份

​	热备份是指在备份期间，数据库对外提供正常服务，在数据库运行中即可备份；冷备份是指在数据库服务停止的情况下，备份物理文件，这种备份最简单，最直接，不过在生产环境下不太实用；温备份也是在数据库服务运行中备份，不过通常加一个全局读锁以保证备份数据的一致性，温备份期间，数据库对外提供非正常服务，只能读取数据，不能写入数据。

​	按照数据备份的范围，可分为：

- 全量备份
- 增量备份
- 日志备份

​	全量备份是指对完整的数据库进行备份。增量备份是指在上次全量备份的基础上，对更改的数据进行备份。日志备份指的是MySQL二进制日志的备份，使用二进制日志备份可进行基于时间点的数据恢复，MySQL数据库主从复制也是基于二进制日志实现的。

​	二进制日志备份不属于真正意义上的增量备份，官方并没有提供真正的增量备份方法，大部分是通过二进制日志来完成增量备份，但这种备份较真正的增量备份来说，效率较低。举个例子，自从上次完整备份以来，数据A先后被修改为B，再修改为C，最后又修改为A，从增量备份的定义上来说，现在时间点与上次完整备份的时间点相比，数据A并没有发生变化，也就不需要增量备份这部分数据。但二进制日志会把这段时间内数据A的所有变化记录在内，备份和恢复时间肯定比增量备份要长一些。Xtrabackup 可以实现真正意义的增量备份。

​	按照数据备份的内容形式，可分为：

- 逻辑备份
- 物理备份



​	逻辑备份的内容是人们可直接阅读的文本，也就是以SQL的形式输出。物理备份



## 11.2 逻辑备份 

从数据备份方式上分，数据库备份分为**逻辑备份**和**物理备份**两种。`mysqldump` 是MySQL自带的逻辑备份工具。

### mysqldump

**语法:** 

```shell
# 备份单库或数据表
mysqldump [OPTIONS] database [tables]
# 备份单/多个数据库
mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
# 备份全部数据库
mysqldump [OPTIONS] --all-databases [OPTIONS]
```

常用选项：

| 参数名                  | 描述                             | 默认值 |
| ----------------------- | -------------------------------- | ------ |
| --host                  | 服务器IP地址                     |        |
| --port                  | 服务器端口号                     |        |
| -u                      | 用户名                           |        |
| -p                      | 密码                             |        |
| -v                      | 打印执行过程信息                 |        |
| --databases             | 指定要备份的数据库               |        |
| --all-databases         | 备份所有的数据库                 |        |
| **--lock-tables**       | 锁定操作数据库中的所有表（读锁） | true   |
| --lock-all-tables       | 锁定所有数据库中的所有表（读锁） | false  |
| --single-transaction    | 整个备份动作放入一个事务中       |        |
| --force                 | 出现错误时继续备份               | false  |
| --compact               | 压缩输出 (节省带宽)              | false  |
| --default-character-set | 指定字符集                       | utf8   |

控制输出语句的选项：

| 参数名              | 描述                                                         | 默认值 |
| ------------------- | ------------------------------------------------------------ | ------ |
| -n                  | 不输出创建数据库语句                                         |        |
| -t                  | 不输出创建表结构语句                                         |        |
| -d                  | 不输出增加表数据语句                                         |        |
| -R                  | 备份函数和存储过程                                           |        |
| --add-drop-database | 在每次创建之前添加一个DROP DATABASE                          | FALSE  |
| --add-drop-table    | 在每次创建之前添加一个DROP TABLE                             | TRUE   |
| --add-locks         | 在INSERT语句周围添加锁。（默认为开，使用--skip-add-locks禁用。） | TRUE   |
| --add-drop-trigger  | 在每次创建之前添加一个DROP TRIGGER                           | FALSE  |

注：更多用法使用 `mysqldump --help` 查询。

:warning: 警告：

​	默认情况下，mysqldump 在备份时会锁定数据库的所有表，直到备份完成。在此期间数据库对外只能读，不能写！所以**生产环境下的数据库，务必不要在有业务的时间段进行  mysqldump 逻辑备份！**因为备份期间会导致数据库服务不能提供正常服务。

​	如果你的服务在夜间或某个时间段几乎没有业务运行，那么可以在这个时间段进行逻辑备份（如 00:00 - 04:00 时间段），如果你的服务必须24小时不间断对外提供正常服务，那么 mysqldump 备份就不合适了，此时应该使用在线物理备份的方式进行数据备份。

​	`--lock-tables` 只是依次锁住操作的数据库中的所有表，因此只能保证单个库中备份数据的一致性，不能保证一个数据库实例下所有数据库的数据一致性。使用 `--lock-all-tables` 选项就可以锁住数据库实例的所有数据库的所有表，保证整个数据库实例备份数据的一致性。



### 备份数据 

```shell
# 备份指定数据表(多个表用空格隔开)
mysqldump -u root -p DB1 table1 table2 > ./backup/backup.sql
# 备份指定数据库
mysqldump -u root -p DB1 > ./backup/DB1.sql
# 备份指定数据库,忽略某些表
mysqldump -u root -p DB1 --ignore-table=DB1.table1 > ./backup/DB1.sql
# 备份所有数据库
mysqldump -u root -p --all-databases > ./backup/all.sql
```

注:

​	备份数据库时忽略某些表的写法是 `--ignore-table=database.table` ，等号左右不能有空格，必须是数据库名.表名；忽略多个表重复写 `--ignore-table` 即可；

### 还原数据

还原逻辑备份的数据非常简单，只需要让mysql执行这些SQL就可以了。SQL所含的数据量越大，还原时间越长。

```shell
# mysqldump 还原数据
mysql -u root -p DB1 < ./backup/DB1.sql
```

注：还原的数据库名必须存在，而且与备份文件的前缀名一致才可以；



**加快还原速度** 

​	使用逻辑备份还原数据库时，实际上是执行大量的 create table 和 insert 语句，mysql 持久化数据的执行机制是从缓冲存储到事务日志文件，再从事务日志文件同步到磁盘上，这一系列操作默认是同步的，可以通过临时调整系统参数，修改为异步执行，从而加快备份速度：

```mysql
set global innodb_flush_log_at_trx_commit = 0;  # 默认为策略1,策略0效率最高;
set global innodb_flush_log_at_timeout    = 3;  # 默认为1秒;
```

如果你的服务开启了 binlog，可以通过关闭 binglog ，或降低刷新频率加快备份速度：

```mysql
set global sync_binlog = 3000;  # 默认为0,修改为3000次写操作才同步刷新到磁盘一次;
```

:warning: 注意：

- ​	数据还原完毕后把参数再修改回来；
- ​	这种追求性能而舍弃安全的做法，尽量不要在生产环境中使用，建议在测试服务或本地服务使用；



## 11.3 物理备份

​	物理备份即备份物理文件，而不是导出逻辑SQL语句。物理备份又分为冷备份和热备份，冷备份是把数据库服务停掉，备份数据文件。热备份是在数据库服务不停止的情况下进行数据备份。

​	mysqldump 是逻辑备份，劣势是备份和恢复的速度较慢，如果数据库很大（如超过50G或100G），mysqldump备份就比较吃力。在数据量很大的情况才，优先使用物理备份。



### Xtrabackup 

Xtrabackup 是 Percona 公司开源的一款数据库热备份工具。在线热备份即备份时不影响数据读写。

官网: https://www.percona.com/software/mysql-database/percona-xtrabackup

**应用：** 

​	很多中小型企业都会选择云数据库（如阿里云），阿里云数据库RDS 的物理备份就是通过 Xtrabackup 实现的。

**对应版本：** 

​	MySQL 5.6及之前的版本需要安装 Percona XtraBackup 2.3 

​	MySQL 5.7版本需要安装 Percona XtraBackup 2.4 

​	MySQL 8.0版本需要安装 Percona XtraBackup 8.0 

**注意事项：**

​	Xtrabackup 目前**仅支持在Linux环境**下备份和恢复数据，官方没有提供其他环境的安装包，所以Windows和Mac OS 暂时无法使用。

使用XtraBackup 实现备份：

```shell
# 全量备份
./xtrabackup --backup --target-dir=/backup/base
# 增量备份
./xtrabackup --backup --target-dir=/backup/base --incremental-basedir=/backup/increment
```



### MySQL Enterprise Backup

​	MySQL 企业级备份方案，支持各种姿势的备份，特点是需要花钱买。



## 11.4 二进制日志备份

​	指定每天 02:00 时分进行一次数据完整备份，如果在18:00 时数据库发生勿删操作（删表或删库），那么想要恢复被删除的数据，只能找到最近一次的完整数据备份并还原，可 02:00 - 18:00 期间的数据就丢失了！所以只有最近数据的完整备份是不够的，还需要备份二进制日志。

​	有了二进制日志和数据完整备份，理论上可以恢复最近任意时间点的数据，也就是说可以做基于时间点的数据恢复。



### 使用binlog恢复数据

binlog恢复数据有两种方式：

- 使用事件时间点进行恢复
- 使用事件位置点进行恢复

我们使用 mysqlbinlog 工具来操作binlog：

```shell
mysqlbinlog [options] log-files
```

**使用事件时间点进行恢复** 

假如在2017年4月20日上午10:00，执行了一个删除的SQL语句。事后要还原表和数据，执行如下命令：

```shell
shell> mysqlbinlog --stop-datetime="2017-04-20 9:59:59" \  
         /var/lib/mysql/mysql-bin.000001 | mysql -u root -p  
```

这样，截止到 "2017-04-20 9:59:59" 时间点的数据就全部恢复了。

如果还需要恢复2017年4月20日上午10:00之后发生的活动。可以用命令：

```shell
shell> mysqlbinlog --start-datetime="2017-04-20 10:01:00" \  
         /var/lib/mysql/mysql-bin.000001 | mysql -u root -p  
```

**使用事件位置点进行恢复**

```shell
shell> mysqlbinlog --stop-position=368312  /var/lib/mysql/mysql-bin.000001 \  
         | mysql -u root -p  
shell> mysqlbinlog --start-position=368315  /var/lib/mysql/mysql-bin.000001 \  
         | mysql -u root -p 
```



binlog 和 redo log 的区别和联系：

​	binlog 记录的是数据的逻辑变化（SQL或以某些符号分割的文本）。redo log 记录的是数据页的物理变化。

​	二进制日志 binlog 是MySQL服务层实现的功能，与存储引擎无关。**binlog优先于事务日志被记录。**



**特别注意：** 

​	**`expire-log-days` 参数应该设置的足够长，至少可以从最近的两次物理备份中做基于时间点的恢复。** 如果数据备份的周期是2天一次，那么 Binlog 的保留天数**至少**是2天。



## 11.5 数据库备份策略

**强烈建议数据备份和binlog 备份都开启！** 
数据库如果数据量很大, 推荐使用物理备份；Mysqldump 逻辑备份期间会锁库，如果不能接受，也要选择物理备份；
推荐备份保留30天, 至少保留7天;
推荐 备份位置为远程主机，而不是本机；
binlog 记录格式为 row (基于行的复制)



逻辑备份注意事项：

​	如果使用 Mysqldump 逻辑备份全量数据，一定要注意备份时间，选在几乎不会产生业务数据的时间段，因为逻辑备份期间，数据库中的所有表是不能写入数据的，也就意味着不能对外提供正常服务。

​	对于数据量很大的库，逻辑备份时常在10分钟、20分钟，甚至是半小时以上都有可能，数据量越大，备库时间越长，对于生产库来说，长时间不能对外访问，其实是糟糕的一件事情，试想一下，如果备库时间为半小时左右，那么MySQL服务的可用性就降低了2%，在一天中有2%的时间是无法正常使用MySQL数据库的。所以对于数据量大的数据库，应该优先使用物理备份。



# 12. 客户端连接与连接池UGtdySjuV



## 客户端连接

**JDBC**

 	Java 数据库连接（Java Database Connectivity），简称JDBC，是Java提供的访问数据库的应用接口。各大数据库厂商提供数据库访问接口的具体实现，打包为一个驱动，供开发人员使用。

​	JDBC 相关的接口定义在 `java.sql` 和 `javax.sql` 中，JDBC 是一组“低级”的基础接口，它用于直接调用SQL。许多ORM框架通过操作对象实现对数据库的增删改查，底层封装的就是JDBC。



JDBC 连接数据库代码：

```java
public static void main(String[] args) throws Exception {
    
	Connection connection = DriverManager.getConnection(url,username,password);
	String sql = "select * from student";
	PreparedStatement prepareStatement = connection.prepareStatement(sql);
	ResultSet rs = prepareStatement.executeQuery();
	while(rs.next()) {
		int id = rs.getInt("id");
		String name = rs.getString("name");
		int age = rs.getInt("age");
	}
}
```



## 数据库连接池

​	与数据库建立连接非常耗时，为每个用户的每次数据库操作建立一个连接是不现实的，为此引入“池化”思想，把宝贵的数据库连接资源保存在内存中，当用户请求时就在连接池中分配一个已有的连接，而不是每次创建新的数据库连接；当数据库操作完毕后，把连接资源放回池子里以供下次操作继续使用，使用数据库连接池可以显著提高访问数据库时的性能。



以Java为例，目前几种主流的数据库连接池 ：

- C3P0 
- DBCP    （Apache 开源项目）
- Druid    （阿里开源项目）



这里以 DBCP  为例，简单说一下重要的配置信息：

```properties
# 基本信息
url: jdbc:mysql://127.0.0.1:3306/jackpot
username: root
password: ***
driverClassName: com.mysql.cj.jdbc.Driver

# 初始化连接数,默认为0
initialSize = 10 
# 可以分配的最大连接数,默认为8
maxTotal = 8 
# 池中可以空闲的最大连接数,默认为8
maxIdle = 8 
# 池中可以空闲的最小连接数,默认为0
minIdle = 2 
# 最大等待毫秒数,默认无限期
maxWaitMillis = 10000
```

关于DBCP 更多其他的连接池配置信息请访问官网文档。

http://commons.apache.org/proper/commons-dbcp/configuration.html



# 13. 数据库服务器状态UGtdySjuV



注: 大部分的系统状态可以通过 SHOW 来查询

## 系统变量

系统变量

```mysql
# 查看所有会话级系统变量(默认查看的是会话级别变量);
SHOW (SESSION) VARIABLES;	
# 查看所有全局级系统变量;
SHOW GLOBAL VARIABLES;	
# 查看系统变量支持模糊查询;
SHOW VARIABLES LIKE '%INNODB%';	
```



关于MySQL的系统参数，有几点需要注意：

1. 大部分参数支持两种配置方式：配置文件和运行时修改系统变量值；
2. 参数的作用范围有全局 global 和 会话 session 两种；
   大部分参数既有全局作用也有会话作用，有的参数只有会话级，没有全局级；
3. 有些参数为只读，只能通过配置文件来指定值；（如log_bin）





## 状态变量



```mysql
show status
```



## 查看连接状态

查看当前的连接列表清单

```mysql
# 查看当前所有连接信息
Show processlist;
# 等价于上面
select * from information_schema.processlist;
```





```mysql
mysql> SELECT * FROM performance_schema.threads;
*************************** 1. row ***************************
          THREAD_ID: 1
               NAME: thread/sql/main
               TYPE: BACKGROUND
     PROCESSLIST_ID: NULL
   PROCESSLIST_USER: NULL
   PROCESSLIST_HOST: NULL
     PROCESSLIST_DB: NULL
PROCESSLIST_COMMAND: NULL
   PROCESSLIST_TIME: 80284
  PROCESSLIST_STATE: NULL
   PROCESSLIST_INFO: NULL
   PARENT_THREAD_ID: NULL
               ROLE: NULL
       INSTRUMENTED: YES
            HISTORY: YES
    CONNECTION_TYPE: NULL
       THREAD_OS_ID: 489803
...
*************************** 4. row ***************************
          THREAD_ID: 51
               NAME: thread/sql/one_connection
               TYPE: FOREGROUND
     PROCESSLIST_ID: 34
   PROCESSLIST_USER: isabella
   PROCESSLIST_HOST: localhost
     PROCESSLIST_DB: performance_schema
PROCESSLIST_COMMAND: Query
   PROCESSLIST_TIME: 0
  PROCESSLIST_STATE: Sending data
   PROCESSLIST_INFO: SELECT * FROM performance_schema.threads
   PARENT_THREAD_ID: 1
               ROLE: NULL
       INSTRUMENTED: YES
            HISTORY: YES
    CONNECTION_TYPE: SSL/TLS
       THREAD_OS_ID: 755399
```



```mysql
# 展示数据库最大连接数的配置
SHOW VARIABLES LIKE 'max_connections';
```

kill 



## 查看线程

mysql 服务中的线程分为**后台线程** （BACKGROUND） **前台线程** （FOREGROUND）两种。

```mysql
# 查看mysql服务中的线程
select * from performance_schema.threads;
```

输出:

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20201123113326910.png" alt="image-20201123113326910" style="zoom:80%;" />





```mysql
# 
thread/sql/main
thread/innodb/io_handler_thread 10 

thread/innodb/srv_lock_timeout_thread
thread/innodb/srv_monitor_thread
thread/innodb/srv_error_monitor_thread
thread/innodb/srv_purge_thread
thread/innodb/srv_master_thread

thread/innodb/page_cleaner_thread
thread/sql/shutdown
# 网络连接线程
thread/sql/con_sockets
thread/sql/one_connection

thread_concurrency	N

innodb_read_io_threads	4
innodb_write_io_threads	4
innodb_purge_threads	1
myisam_repair_threads	1

thread_handling	one-thread-per-connection
```



## InnoDB状态



```mysql
# 显示innodb存储引擎状态的大量信息，包含死锁日志
SHOW ENGINE INNODB STATUS ;
```



输出解读:



## 复制状态



```mysql
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000004 |      120 | jackpot      |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.03 sec)
```





## Information_schema

Information_schema 数据库保存了数据库服务的基本信息



```mysql
# 查询数据表的所有列的属性
SELECT
	COLUMN_NAME,
	column_comment 
FROM
	INFORMATION_SCHEMA.COLUMNS 
WHERE
	table_name = 'table_name' 
	AND table_schema = 'db_name';
```



## performance_schema

性能监控仪表盘

到mysql5.6 performance_schema 库已有50余张表，这表示有50多种数据监控，对服务器的性能是有一定的开销；





## 监测网络

netstat



## 监测磁盘I/O

iostat







# 14. 数据库运维UGtdySjuV



## 用户管理



```mysql
# 查看所有的用户信息
select * from mysql.user
# 创建用户
CREATE USER 'user_name'@'localhost' IDENTIFIED BY 'password';
```

用户的表达语法：user_name@host_name 





## 权限管理

​	Mysql 的每种操作都代表一种权限（privilege），授权（grant）给相应的用户。

使用 `SHOW PRIVILEGES` 查看数据库支持的权限类型:

```mysql
mysql> SHOW PRIVILEGES;
+-------------------------+----------------------------------+------------------------------------+
| Privilege               | Context                          | Comment                               
+-------------------------+----------------------------------+------------------------------------+
| Alter                   | Tables                           | To alter the table                      
| Alter routine           | Functions,Procedures             | To alter or drop stored functions/procedures 
| Create                  | Databases,Tables,Indexes         | To create new databases and tables
| Create routine          | Databases                        | To use CREATE FUNCTION/PROCEDURE
| Create temporary tables | Databases                        | To use CREATE TEMPORARY TABLE
| Create view             | Tables                           | To create new views
| Create user             | Server Admin                     | To create new users
| Delete                  | Tables                           | To delete existing rows
| Drop                    | Databases,Tables                 | To drop databases, tables, and views
 ......
```

注：mysql5.6 支持30 多种权限类型；

根据操作对象的不同，作用域可分为 数据库、数据表、索引、函数、存储过程、服务器 等几种；



Mysql 的授权用户由两部分组成：登录主机名和用户名。



### 查看用户及权限

```mysql
# 查看所用用户-权限信息
select * from mysql.user
# 查看某个用户的授权信息
show grants for 'jackpot'@'localhost';
```



| 权限         | 作用域 | 描述 |
| ------------ | ------ | ---- |
| create       |        |      |
| drop         |        |      |
| alter        |        |      |
| show view    |        |      |
| create view  |        |      |
| insert       |        |      |
| delete       |        |      |
| update       |        |      |
| select       |        |      |
| index        |        |      |
| trigger      |        |      |
| grant option |        |      |
| reference    |        |      |



skip-grant-tables

The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement

### 授予权限

grant 关键字用于给用户授权.

```mysql
# 给用户授权
GRANT ALL PRIVILEGES ON *.* TO 'jackpot'@'localhost' IDENTIFIED BY PASSWORD '*6AE236078C8F42F6FDA2C35E08941CC6D17204D7'
WITH GRANT OPTION
```



 `ON *.*`  表示权限作用于所有数据库的所有表;

`TO user@host` 权限授权给指定用户;

`WITH GRANT OPTION` 是固定语法;

注:

​	在授权时填写的 password 字符格式，必须是加密后的密文格式，不能是明文，否则报错；可以预先使用 password（）函数将密码处理后再填写。

```mysql
select PASSWORD('www2020');
```



### 回收权限

revoke 关键字用于给用户回收权限.

```mysql
revoke 
```



刷新权限：

```mysql
FLUSH PRIVILEGES; 
```





## 管理MySQL命令

数据库

```mysql
show databases;		-- 列出MySQL服务的所有数据库列表;
use database_name;	-- 选择要操作的数据库,使用该命令后所有的mysql命令都只针对该数据库;
show tables;		-- 查看某个数据库的所有表;
```

数据表

```mysql
DESC table_name;				-- 查看指定数据表的字段信息:字段名,类型,默认值,是否非空约束
DESCRIBE table_name;			-- 同上；
SHOW COLUMNS FROM table_name;	-- 同上；

show table status like 'BMS_MD_GOODS';	-- 查看表状态: 存储引擎,索引,数据碎片,字符集,创建时间...
SHOW INDEX FROM table_name;				-- 查看指定数据表的索引信息
```

实用SQL:

```mysql
# 查看线程处理列表
SHOW PROCESSLIST;

# 显示innodb存储引擎状态的大量信息，包含死锁日志
SHOW ENGINE INNODB STATUS ;
# 展示数据库最大连接数的配置
SHOW VARIABLES LIKE 'max_connections';
# 查看存在哪些触发器
SELECT * FROM information_schema.TRIGGERS;
# 查看MySQL版本
SELECT VERSION();
```





## 监控MySQL服务的运行状态

​	有一次某个服务突然挂了，原因不详，重启业务服务器时，一直处于启动中，



```mysql
# 查看mysql工作处理列表 (目前每个连接的情况)
show PROCESSLIST;
# 等同于上面
select * from information_schema.processlist;
# 查看当前运行线程
select * from performance_schema.threads;
# 查看当前的连接ID
select CONNECTION_ID();
```





## 调整接收最大网络包的大小

**max_allowed_packet** 允许最大的网络包大小

```mysql
mysql> show variables like '%max_allowed_packet%';
+--------------------------+------------+
| Variable_name            | Value      |
+--------------------------+------------+
| max_allowed_packet       | 4194304    |
| slave_max_allowed_packet | 1073741824 |
+--------------------------+------------+
```



如果写入的值大小超过mysql运行的最大网络包大小，系统会报错：

```mysql
Packet for query is too large (14004196 > 11999232). You can change this value on the server by setting the max_allowed_packet' variable.
```

此时需要适当调大 `max_allowed_packet` 参数的值。



## 加锁 | 解锁数据库

加锁和解锁数据库操作常在备份数据库时使用。

手动锁数据库:

```mysql
# 切换数据库;
use db1;
# 给db1数据库加读锁,所有表的写操作全部阻塞;
flush tables with read lock;		
# 解锁;
unlock tables;					
```

手动锁表:

```mysql
LOCK TABLES `student` read ;	# 给某个表加 读锁: 其他事务允许读,不允许写;
LOCK TABLES `student` write ;	# 给某个表加 写锁: 其他事务的读和写都不允许;
# 解锁;
UNLOCK TABLES;
```

## 导入导出数据



```mysql
# 
select * from goods
INTO OUTFILE "/var/lib/mysql-files/goods.sql"
```



如果导出失败,查看错误日志,很有可能是输出目录有限制.

```mysql
# 查看mysql服务导出文件的限制目录 ：默认值为 /var/lib/mysql-files/
SHOW VARIABLES LIKE "%secure_file_priv%";
```

secure_file_priv 参数可以有3类值：

- null 表示不允许mysql服务导入导出数据文件；
- 空值表示不限制mysql服务导入导出数据文件的位置；（注意等号 = 不能省略）
- /home 表示限制mysql服务导入导出数据文件的位置只能是 /home ；

secure_file_priv 是一个只读参数，不能通过 set 命令设置，只能在mysql配置文件中设置，并重启服务才能生效。



## Percona ToolKit

Percona ToolKit 是 Percona 公司开发的管理MySQL的高级**命令行工具集**，开源免费。



## 释放删除空间

场景：

​	删除表数据，delete完以后，表空间文件并不会减少，这是因为碎片空间的存在。可以使用 `optimize table`  对表做碎片整理来释放空间。

```sql
optimize table BMS_MD_GOODS;
```

注意:

​	`optimize` 命令会进行**锁表操作**！禁止原表的写操作，只允许读。所以进行优化要避开数据操作时间，避免事故发生。



# 15. 事件（Event）UGtdySjuV

MySQL的事件是一个定时任务，可以是一次性任务，也可以是周期性任务。

事件定义在数据库中，不是数据表的操作对象，可以通过 `SHOW EVENTS` 查看当前数据库定义的所有事件。

事件功能默认关闭，可以通过配置文件或设置全局变量进行开启：

```mysql
-- 查看是否开启事件功能
SHOW VARIABLES LIKE 'event_scheduler';
-- 开启事件功能
SET GLOBAL event_scheduler = ON;
```

常用事件操作语法：

```mysql
-- 查看当前数据库定义的所有事件
SHOW EVENTS;
-- 查询指定数据库创建的所有事件
select * from mysql.event where db = 'db_name';
-- 启用指定事件任务
ALTER EVENT event1 ENABLE;
-- 禁用指定事件任务
ALTER EVENT event1 DISABLE;
-- 查看事件的详情创建信息
SHOW CREATE EVENT event1;
```



## 15.1 周期性事件

```SQL
CREATE EVENT event_name		
ON SCHEDULE EVERY 60 MINUTE_SECOND	-- 周期性任务：每60秒执行一次
STARTS '2020-06-02 14:00:00.000' ENDS '2021-06-01 14:00:00.000'
ON COMPLETION PRESERVE 
DISABLE
DO
BEGIN
insert into bms_md_goods (stdcode) values ('01');
insert into bms_md_goods (stdcode) values ('02');
END;
```

​	第一行指定事件名称，第二行指定事件是周期性任务，并指定频率为每60秒执行一次，第三行指定事件执行的开始时间和结束时间，第四行指定事件执行完成后保存该事件，第五行设置事件的启用状态；

​	DO 关键字后面跟的是事件执行体；

## 15.2 一次性事件

```SQL
CREATE EVENT event_name		
ON SCHEDULE AT '2020-06-02 14:00:00.000' -- 一次性任务
ON COMPLETION NOT PRESERVE 
ENABLE
DO
insert into bms_md_goods (stdcode) values ('01');
```



**语法名词解释:**

event_name:  	事件名称

ON SCHEDULE：

​	计划任务，后面指定事件执行的频率和时间，频率有两个选项：`AT` 和 `EVERY`。AT 表示一次性任务，后面指定任务执行时间； EVERY 表示周期性任务，后面指定频率和执行时间， STARTS 指定开始时间，ENDS 指定结束时间；

ON COMPLETION [NOT] PRESERVE：

​	可选项，默认是 `ON COMPLETION NOT PRESERVE` ，即任务完成后不保存，删除事件； `ON COMPLETION PRESERVE` 则不会删除掉。

ENABLE | DISABLE：

​	事件的开启状态，默认开启，即 `ENABLE` 状态，`DISABLE` 表示关闭状态。可以通过 `ALTER EVENT` 修改事件开启状态；

DO event_body:

​	`DO` 后面是事件执行体，如果执行多条SQL语句，使用 `BEGIN` 和 `END` 包起来；



# 16. 触发器（Trigger）UGtdySjuV

触发器是建立在表上的操作对象。

## 16.1 基本语法

语法：

```mysql
CREATE TRIGGER trigger_name 
trigger_time trigger_event ON table_name 	# 指定触发时机,触发事件,表名: before insert on student
FOR EACH ROW								# 以数据行为单位进行触发,固定写法;
BEGIN
	trigger_exec_body 
END;
```

属性说明:

​	trigger_name		触发器名称

​	table_name   		触发器的表名

​	trigger_event   	 触发事件，有三个可选项：insert / delete / update 

​	trigger_time    	  触发时机，有两个可选项：before / after

​	trigger_exec_body 	触发器执行体



注：

- 触发器的执行体不能返回结果集；
- 触发器是以数据行为基本单位进行触发的；

## 16.2 示例

下面的示例表示在 职员 staff 表上创建触发器，名称为 staff_user，触发时机在一行数据新增后，往用户表 user 插入一条对应的数据。

```mysql
CREATE TRIGGER staff_user 
after insert  ON staff 
FOR EACH ROW								
BEGIN
	insert into `user` (id,`name`,age) VALUES (new.id,new.`name`,new.age); 
END;
```



## 16.3 NEW 和 OLD

MySQL定义了 NEW 和 OLD 关键字，用于表示触发**触发器本身的那行数据**。

- 在 INSERT 触发器中，NEW 表示即将（BEFORE）或已经（AFTER）插入的新数据；
- 在 DELETE 触发器中，OLD 表示即将或已经删除的旧数据；
- 在 UPDATE 触发器中，OLD 表示即将或已经更新的旧数据；NEW 表示即将或已经更新的新数据；

注：

​	OLD 是只读的，而 NEW 则可以使用 SET 赋值。

## 16.4 查看触发器

```mysql
# 查看当前使用数据库的所有触发器
SHOW TRIGGERS;
# 查看指定的触发器
SHOW CREATE TRIGGER trigger_name;
# 查看数据库实例的所有触发器
SELECT * FROM information_schema.TRIGGERS;
```

## 16.5 尽量避免使用触发器

​	触发器在实际应用中并不广泛，甚至可以说很多开发人员都不会使用数据库提供的触发器，原因大致有这么几点：

- 触发器本身不容易维护，并且触发器因为是隐式调用，容易被忽略；
- 在并发度高的业务场景下，会给数据库服务带来更大的压力，带来系统性能瓶颈；



# 17. 存储过程（Procedure）UGtdySjuV

存储过程的思想很简单，就是数据库在SQL语言层面的代码封装与重用。

**创建语法** 

```mysql
CREATE PROCEDURE procedure_name (arg...)
BEGIN
exec_body
END
```

参数列表

​	参数列表可以填写0或多个参数，语法是 [IN|OUT|INOUT] 参数名 数据类型。

注: 确保参数的名字不要和表的列名相同，否则按列名处理。

**调用** 

```mysql
CALL procedure_name (arg...);
```

**查看** 

```mysql
# 查看指定数据库的存储过程
SHOW PROCEDURE STATUS WHERE db= db_name;
# 查看存储过程的详情创建信息
SHOW CREATE PROCEDURE procedure_name;
```



## 缺点

MySQL的存储过程在实际应用中也不广泛，原因大致有这么几点：

- 存储过程往往定制化于特定的数据库上，当切换到其他数据库时，需要重写原有的存储过程；
- 存储过程的性能调试，受限于各种数据库系统；



《阿里巴巴Java开发手册》 Mysql规约部分对存储过程有强制要求：

【强制】禁止使用存储过程，存储过程难以调试和拓展，更没有移植性。





# 18. 其他UGtdySjuV

MySQL的data目录是专门存储数据的地方，data目录主要有以下几种文件：

**.frm**

​	存储表结构的数据。视图也会以.frm的格式进行存储。

**.ibd**

​	存储表数据和索引数据。

**db.opt**

​	存储数据库的默认字符集和排序规则。

----



**最小存储单位**

​	我们知道，在计算机中，表示信息的最小单位是位 bit，1 bit代表一个0 或 1 。就像当今社会最小的货币流通单位是分。

​	比位稍大一点的单位是字节。1字节=8位。

​	**磁盘**的最小存储单位是扇区（sector），一个扇区 = 512字节。
​	操作系统依靠**文件系统**存储数据，文件系统并不是一个扇区一个扇区的读写数据，那样效率很低，为此文件系统引入块（block）的概念，最小存储单位是块 block，1 块 = 4096字节（4k） 。

​	**数据库**系统是专门用来存储数据的服务，数据库操作数据的最小单位是页（page），以mysql的Innobd存储引擎为例，1页 = 16k ，

- 磁盘物理扇区（sector）：512字节
- 文件系统的块（block） ：4K
- 数据库的页：（page） ：16K

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200527141632202.png" alt="image-20200527141632202" style="zoom:80%;" />

innodb 存储数据的最小单位是页，1页的大小默认是16K （16384字节）；

可以通过 innodb_page_size 参数查看到页大小：

```shell
mysql> show global VARIABLES like '%innodb_page_size%';
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| innodb_page_size | 16384 |
+------------------+-------+
1 row in set (0.02 sec)
```

所有 .idb 的数据文件大小都是 16K的整数倍，.frm 的表结构文件因为不会很大，所以是按普通文件形式进行存储的。

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200527092639490.png" alt="image-20200527092639490" style="zoom:80%;" />



​	说到这里，不禁要提一下CPU缓存的设计理念，在主流的CPU上，有三级高速缓存：L1、L2、L3，距离CPU核心越近，速度越快，容量越小。L1速度最快，越来越接近CPU的处理速度，L3的速度最慢，但也比主存的速度快很多倍。



<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/image-20200715095229689.png" alt="image-20200715095229689" style="zoom:80%;" />

​	对于CPU来说，也不是一个字节一个字节的从高速缓存获取数据，因为这样效率很低，也是一块一块的的加载的，这样的一块数据称之为“Cache Line”，1 Cache Line = 64 bytes（相当于16个int类型的变量），这就是CPU操作高速缓存的最小数据单位。

​	可以看到，CPU的缓存行，文件系统的块，数据库的页，都利用了局部性原理而设计出来的，这种局部性原理贯穿整个计算机科学。



# READMEUGtdySjuV

版权声明：

​	本文遵循**[知识共享许可协议3.0（CC 协议）](https://creativecommons.net.cn/licenses/meet-the-licenses/)**： [署名-非商业性使用-相同方式共享 (by-nc-sa)](https://creativecommons.org/licenses/by-nc-sa/3.0/cn/) 

作者：

​	米开

微信公众号：**米开的网络私房菜** 

<img src="https://gitee.com/Jackpotsss/pic_go/raw/master/img/QRcode.png" alt="image-20200715095229689"  />

参考：

   《高性能Mysql》第三版

   《阿里巴巴Java开发手册》 Mysql规约部分

​	[MySQL官方文档](https://dev.mysql.com/doc) 

​	微信公众号: <SQL数据库开发>

​	[MySQL索引原理及慢查询优化](https://tech.meituan.com/2014/06/30/mysql-index.html) 

​	[如何避免回表查询？什么是索引覆盖？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651962609&idx=1&sn=46e59691257188d33a91648640bcffa5&chksm=bd2d092d8a5a803baea59510259b28f0669dbb72b6a5e90a465205e9497e5173d13e3bb51b19&mpshare=1&scene=1&srcid=&sharer_sharetime=1564396837343&sharer_shareid=7cd5f6d8b77d171f90b241828891a85f&key=abd60b96b5d1f2e52ca45314fb2c95a67fad7a457fe265562eb51a1c026389d3f28c52359f96e920368ab44a5d08ebcbbe2ded474be2ba70731ed8b5dcc5dd68cc0eceb4989a74fb04e5055c78af8d38&ascene=1&uin=MTAwMjA4NTM0Mw%3D%3D&devicetype=Windows+7&version=62060739&lang=zh_CN&pass_ticket=tXA4xc7SZYamLpGZz5B6JwJa1ZRvZ4bRlmzFhXwEKeOfloPLulU0O80gsIQUiONb)



修改记录：

2020-02-08	米开	第一次修订	

2020-02-13	米开	增加索引相关的内容 

2020-03-10	米开	增加间隙锁相关的内容  

2020-03-12	米开	增加 redo log / undo log / bin log 相关的内容  

2020-05-10    米开	增加 binlog 和主从复制相关的知识点	

2020-06-06   米开	 修改约束相关内容，增加触发器，事件内容

2020-06-15   米开	 增加NULL值判断使用索引情况的描述

2020-07-26   米开	第一版 v0.95.0 定稿

2020-09-10   米开	完善备份、恢复章节内容

2020-09-24   米开	第二版 v0.96.0 定稿

2020-09-27   米开	增加用户-权限相关内容，状态监控

2020-10-24   米开	美化代码结构，修改排版，完善SQL基础、数据库备份方面的内容