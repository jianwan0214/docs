## MO 兼容 mysql-dump语句

### 1、mysql-dump 生成语句
参考MySQL 8.0 中关于mysqldump的介绍，https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html

mysqldump 选项
| Value | description |
| 选项名称 | 描述 | 介绍 | 弃用 |
|:-:|:-:|:-:|:-:|
| --添加删除数据库 | 在每个 CREATE DATABASE 语句之前添加 DROP DATABASE 语句 |  |   |
