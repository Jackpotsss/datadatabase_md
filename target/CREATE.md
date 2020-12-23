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



