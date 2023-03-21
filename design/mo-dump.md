## MO 兼容 mysql-dump语句

### 1、mysql-dump 生成语句
参考MySQL 8.0 中关于mysqldump的介绍，https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html

mysqldump 选项
| Value | description |
| 选项名称 | 描述 | 介绍 | 弃用 |
|:-:|:-:|:-:|:-:|
| --添加删除数据库 | 在每个 CREATE DATABASE 语句之前添加 DROP DATABASE 语句 |||
| --添加删除表 | 在每个 CREATE TABLE 语句之前添加 DROP TABLE 语句 |||
| --add-drop-trigger | 在每个 CREATE TRIGGER 语句之前添加 DROP TRIGGER 语句 |||
| --添加锁 | 用 LOCK TABLES 和 UNLOCK TABLES 语句围绕每个表转储 |||
| --apply-replica-statements | 在 CHANGE REPLICATION SOURCE TO 语句之前包括 STOP REPLICA 并在输出结束时包括 START REPLICA |||
