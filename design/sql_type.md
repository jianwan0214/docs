# sql语句的执行来源分析

## 1、sql语句的执行来源分类（内部和外部）
对于MO数据库中，trace中记录的执行的sql语句很多，基本可以分为两大类，一类是外部输入的sql，一类是MO自己内部生成的sql语句；对于这两类，可以很容易的区分开。

这里给出的方案是，在statement_info表中新增一列用于标记该sql语句的来源,
| Field | Type | Null | Key | Default | Extra | Comment |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| sql_type | VARCHAR(36) | YES | | "internal_sql" | |  sql statement source type |

其中sql_type的值取字符串枚举值"internal_sql", "external_sql"。
| Value | description |
|:-:|:-:|
| "internal_sql" | MO内部生成执行的sql|
| "external_sql" | 外部传入MO执行的sql |
下表是statement_info中已有的列信息。

```
>>> show columns from statement_info;
+-----------------------+-----------------+------+------+-----------+-------+--------------------------------------------------------------+
| Field                 | Type            | Null | Key  | Default   | Extra | Comment                                                      |
+-----------------------+-----------------+------+------+-----------+-------+--------------------------------------------------------------+
| transaction_id        | VARCHAR(36)     | YES  |      | '0'       |       | txn uniq id                                                  |
| statement_id          | VARCHAR(36)     | YES  |      | '0'       |       | statement uniq id                                            |
| account               | VARCHAR(1024)   | NO   |      | NULL      |       | account name                                                 |
| database              | VARCHAR(1024)   | NO   |      | NULL      |       | what database current session stay in.                       |
| duration              | BIGINT UNSIGNED | YES  |      | '0'       |       | exec time, unit: ns                                          |
| err_code              | VARCHAR(1024)   | YES  |      | '0'       |       |                                                              |
| error                 | TEXT            | NO   |      | NULL      |       | error message                                                |
| exec_plan             | JSON            | NO   |      | NULL      |       | statement execution plan                                     |
| host                  | VARCHAR(1024)   | NO   |      | NULL      |       | user client ip                                               |
| node_type             | VARCHAR(64)     | YES  |      | 'node'    |       | node type in MO, val in [DN, CN, LOG]                        |
| node_uuid             | VARCHAR(36)     | YES  |      | '0'       |       | node uuid, which node gen this data.                         |
| rows_read             | BIGINT UNSIGNED | YES  |      | '0'       |       | rows read total                                              |
| statement             | TEXT            | NO   |      | NULL      |       | sql statement                                                |
| status                | VARCHAR(32)     | YES  |      | 'Running' |       | sql statement running status, enum: Running, Success, Failed |
| user                  | VARCHAR(1024)   | NO   |      | NULL      |       | user name                                                    |
| statement_fingerprint | TEXT            | NO   |      | NULL      |       | note tag in statement(Reserved)                              |
| response_at           | DATETIME        | NO   |      | NULL      |       | response send datetime                                       |
| session_id            | VARCHAR(36)     | YES  |      | '0'       |       | session uniq id                                              |
| bytes_scan            | BIGINT UNSIGNED | YES  |      | '0'       |       | bytes scan total                                             |
| request_at            | DATETIME        | NO   |      | NULL      |       | request accept datetime                                      |
| statement_tag         | TEXT            | NO   |      | NULL      |       | note tag in statement(Reserved)                              |
+-----------------------+-----------------+------+------+-----------+-------+--------------------------------------------------------------+
21 rows in set (0.04 sec)
```

## 2、外部sql语句的执行来源分类
对于外部来源的sql语句，按照其外部来源可大致分类如下：
mysql client连接，云平台程序，JDBC连接，python-mysql框架连接，go-mysql框架连接，就是需要区分不同的外部连接来源。
对于以上的不同外部连接来源，

