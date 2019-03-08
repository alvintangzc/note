# 修改主键引起的Mysql“死锁”
## 一、事件起因

在一个事务里，A链接select for update锁定一行记录，B链接select for update等待A提交。但是这时候A修改了记录的主键。导致B链接返回：

>  Deadlock found when trying to get lock; try restarting transaction 

## 二、问题分析

使用命令：

> show engine innodb status;

找到 LATEST DETECTED DEADLOCK 部分，发现B链接被rollback了。

新建表测试：

```
CREATE TABLE `t_test_info` (
  `Fcode` varchar(12) NOT NULL ,
  `Fscene` varchar(32) NOT NULL,
  `Fcreate_time` datetime,
  `Fmodify_time` datetime,
  PRIMARY KEY (`Fcode`),
  UNIQUE KEY `uk_scene` (`Fscene`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

插入一条记录：

```
insert into t_test_info values("111","test",now(),now());
```

A,B链接：

```
begin;
select * from t_test_info where Fscene='test' for update;
```

A链接

```
update t_test_info set Fcode='222' where Fcode="111";
```

然后B链接报：

```
ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction
```

##三、总结

前同事建表把易变字段设置成了主键，对于该表应该添加自增主键，或者使用不易改变的字段Fscene做主键，把Code做唯一索引。
