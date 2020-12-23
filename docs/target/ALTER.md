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



