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



