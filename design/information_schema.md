# information_schema MO与mysql异同

## 1、tables表

| 列名 | mysql中列定义 | MO中列定义 | MO中是否已实现 |
|:-:|:-:|:-:|:-:|
| TABLE_CATALOG | varchar(64) | VARCHAR(0) | 未实现 |
| TABLE_TYPE | enum('BASE TABLE','VIEW','SYSTEM VIEW')  | VARCHAR(0) | 未实现 |
| ENGINE | varchar(64) | VARCHAR(0) | 未实现 |
| VERSION | varchar(64) | VARCHAR(0) | 未实现 |
| ROW_FORMAT | enum('Fixed','Dynamic','Compressed','Redundant','Compact','Paged') | VARCHAR(0) | 未实现 |
| TABLE_ROWS | bigint unsigned | BIGINT | 未实现 |
| AVG_ROW_LENGTH | bigint unsigned | BIGINT | 未实现 |
| DATA_LENGTH | bigint unsigned | BIGINT | 未实现 |
| MAX_DATA_LENGTH | bigint unsigned | BIGINT | 未实现 |
| INDEX_LENGTH | bigint unsigned | BIGINT | 未实现 |
| DATA_FREE | bigint unsigned | BIGINT | 未实现 |
| AUTO_INCREMENT | bigint unsigned | BIGINT | 未实现 |
| UPDATE_TIME | datetime | VARCHAR(0) | 未实现 |
| CHECK_TIME | datetime | VARCHAR(0) | 未实现 |
| TABLE_COLLATION | varchar(64) | VARCHAR(0) | 未实现 |
| CHECKSUM | bigint | BIGINT | 未实现 |
| CREATE_OPTIONS | varchar(256) | VARCHAR(0) | 未实现 |


## 2、columns表
| 列名 | mysql中列定义 | MO中列定义 | MO中是否已实现 |
|:-:|:-:|:-:|:-:|
| TABLE_CATALOG | varchar(64) | VARCHAR(3) | 未实现 |
| NUMERIC_PRECISION | bigint unsigned | BIGINT | 未实现 |
| NUMERIC_SCALE | bigint unsigned | BIGINT | 未实现 |
| DATETIME_PRECISION | int unsigned | BIGINT | 未实现 |
| CHARACTER_SET_NAME | varchar(64) | VARCHAR(4) | 未实现 |
| COLLATION_NAME | varchar(64) | VARCHAR(8) | 未实现 |
| GENERATION_EXPRESSION | longtext | VARCHAR(0) | 未实现 |
| SRS_ID | int unsigned | BIGINT | 未实现 |


## 3、schemata表
| 列名 | mysql中列定义 | MO中列定义 | MO中是否已实现 |
|:-:|:-:|:-:|:-:|
| DEFAULT_CHARACTER_SET_NAME | varchar(64) | VARCHAR(7) | 未实现 |
| DEFAULT_COLLATION_NAME | varchar(64) | VARCHAR(18) | 未实现 |
| SQL_PATH | binary(0) | VARCHAR(0) | 未实现 |
| DEFAULT_ENCRYPTION | varchar(64) | VARCHAR(2) | 未实现 |
