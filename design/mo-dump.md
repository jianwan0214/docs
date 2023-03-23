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
| USE DATABASE |
| DROP TABLE |
| CREATE TABLE |
| INSERT INTO TABLE |
| REPLACE SQL |

### 2、CREATE DATABASE 语法
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

### 3、DROP DATABASE 语法
```
DROP DATABASE [ IF EXISTS ] <数据库名>
语法说明如下：
<数据库名>：指定要删除的数据库名。
IF EXISTS：用于防止当数据库不存在时发生错误。
DROP DATABASE：删除数据库中的所有表格并同时删除数据库。使用此语句时要非常小心，以免错误删除。如果要使用 DROP DATABASE，需要获得数据库 DROP 权限。
```


可以使用SHOW ENGINES;语句查看系统所支持的引擎类型，结果如下所示。
```
mysql> SHOW ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| ndbcluster         | NO      | Clustered, fault-tolerant tables                               | NULL         | NULL | NULL       |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| ndbinfo            | NO      | MySQL Cluster system information storage engine                | NULL         | NULL | NULL       |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
11 rows in set (0.00 sec)
```

### 4、CREATE TABLE 语法
```
CREATE TABLE <表名> ([表定义选项])[表选项][分区选项];
其中，[表定义选项]的格式为：
<列名1> <类型1> [,…] <列名n> <类型n>
CREATE TABLE 命令语法比较多，其主要是由表创建定义（create-definition）、表选项（table-options）和分区选项（partition-options）所组成的。
```

### 5、INSERT INTO TABLE 语法
INSERT 语句有两种语法形式，分别是 INSERT…VALUES 语句和 INSERT…SET 语句。  
1、INSERT ... VALUES语句  
INSERT VALUES 的语法格式为：
```
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];

语法说明如下。
<表名>：指定被操作的表名。
<列名>：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT<表名>VALUES(…) 即可。
VALUES 或 VALUE 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。
```

2、INSERT…SET语句  
语法格式为：
```
INSERT INTO <表名>
SET <列名1> = <值1>,
    <列名2> = <值2>,
    …
```

### 6、REPLACE 语法
```
MySQL REPLACE语句是标准SQL的MySQL扩展。MySQL REPLACE语句的工作原理如下：

如果新行已不存在，则MySQL REPLACE  语句将插入新行。
如果新行已存在，则  REPLACE  语句首先删除旧行，然后插入新行。在某些情况下，REPLACE语句仅更新现有行。
要确定表中是否已存在新行，MySQL使用PRIMARY KEY或UNIQUE KEY 索引。如果表没有这些索引之一，则REPLACE语句等同于INSERT语句。

使用REPLACE语句时需要了解几个要点：

如果您开发的应用程序不仅支持MySQL数据库，还支持其他关系数据库管理系统（RDBMS），则应避免使用REPLACE语句，因为其他RDBMS可能不支持它。相反，您可以在事务中使用DELETE  和  INSERT语句的组合  。
如果您使用的是REPLACE在具有TABLE语句触发器和重复键错误发生的删除，触发器将按照以下顺序被解雇：BEFORE INSERT BEFORE DELETE，AFTER DELETE，AFTER INSERT  万一REPLACE语句删除当前行并插入新行。如果REPLACE语句更新当前行，则会触发BEFORE UPDATE和AFTER UPDATE触发器。
```
对于REPLACE语句，目前MO只考虑将其做成insert的别名去实现。
