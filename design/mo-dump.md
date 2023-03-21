## MO 兼容 mysql-dump语句

### 1、mysql-dump 生成语句
参考MySQL 8.0 中关于mysqldump的介绍，https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html
mysqldump客户端实用程序执行 逻辑备份，生成一组 SQL 语句，可以执行这些语句以重现原始数据库对象定义和表数据。它转储一个或多个 MySQL 数据库以进行备份或传输到另一台 SQL 服务器。mysqldump命令 还可以生成 CSV、其他分隔文本或 XML 格式的输出。

mysqldump 选项
| Value | description |
| 选项名称 | 描述 | 介绍 | 弃用 |
|:-:|:-:|:-:|:-:|
| --add-drop-database | 在每个 CREATE DATABASE 语句之前添加 DROP DATABASE 语句 |||
| --add-drop-table | 在每个 CREATE TABLE 语句之前添加 DROP TABLE 语句 |||
| --add-drop-trigger | 在每个 CREATE TRIGGER 语句之前添加 DROP TRIGGER 语句 |||
| --add-locks | 用 LOCK TABLES 和 UNLOCK TABLES 语句围绕每个表转储 |||
| --apply-replica-statements | 在 CHANGE REPLICATION SOURCE TO 语句之前包括 STOP REPLICA 并在输出结束时包括 START REPLICA | 8.0.26 ||
| --apply-slave-statements | 在 CHANGE MASTER 语句之前包括 STOP SLAVE 并在输出结束时包括 START SLAVE || 8.0.26 |
| --compatible | 生成与其他数据库系统或旧版 MySQL 服务器更兼容的输出 |||
| --create-options | 在 CREATE TABLE 语句中包含所有 MySQL 特定的表选项 |||
| --insert-ignore | 编写 INSERT IGNORE 而不是 INSERT 语句 |||
| --replace | 编写 REPLACE 语句而不是 INSERT 语句 |||
| --set-charset | 将 SET NAMES default_character_set 添加到输出 |||

以上列举出了对于mysqldump时可以设置的选项，这些选项也影响到最终产生的dump文件的内容，对于目前MySQL dump出来的文件，目前看到的语句类型只有如下几类：
| SQL类型 |
|:-:|
| CREATE DATABASE |
| DROP DATABASE |
| DROP TABLE |
| CREATE TABLE |
| INSERT INTO TABLE |
