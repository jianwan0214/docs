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
| REPLACE SQL |

### 1、CREATE DATABASE 语法
```
CREATE DATABASE [IF NOT EXISTS] <数据库名>
[[DEFAULT] CHARACTER SET <字符集名>] 
[[DEFAULT] COLLATE <校对规则名>];

[ ]中的内容是可选的。语法说明如下：
<数据库名>：创建数据库的名称。MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，不能以数字开头，尽量要有实际意义。注意在 MySQL 中不区分大小写。
IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时才能执行操作。此选项可以用来避免数据库已经存在而重复创建的错误。
[DEFAULT] CHARACTER SET：指定数据库的字符集。指定字符集的目的是为了避免在数据库中存储的数据出现乱码的情况。如果在创建数据库时不指定字符集，那么就使用系统的默认字符集。
[DEFAULT] COLLATE：指定字符集的默认校对规则。
```


