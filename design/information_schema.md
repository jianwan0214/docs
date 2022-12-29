# information_schema MO与mysql异同

## 1、tables表

| 列名 | mysql中列定义 | MO中列定义 | MO中是否已实现 |
|:-:|:-:|:-:|:-:|
| TABLE_CATALOG | varchar(64) | VARCHAR(0) | 未实现 |
| TABLE_SCHEMA | varchar(64) | VARCHAR(5000) | 已实现 |
| TABLE_NAME | varchar(64) | VARCHAR(5000) | 已实现 |
| TABLE_TYPE | enum('BASE TABLE','VIEW','SYSTEM VIEW')  | VARCHAR(0) | 未实现 |
| ENGINE | varchar(64) | VARCHAR(0) | 已实现 |
| VERSION | varchar(64) | VARCHAR(0) | 未实现 |
| ROW_FORMAT | enum('Fixed','Dynamic','Compressed','Redundant','Compact','Paged') | VARCHAR(0) | 未实现 |
| TABLE_ROWS | bigint unsigned | BIGINT | 已实现 |
| AVG_ROW_LENGTH | bigint unsigned | BIGINT | 已实现 |
| DATA_LENGTH | bigint unsigned | BIGINT | 已实现 |
| MAX_DATA_LENGTH | bigint unsigned | BIGINT | 已实现 |
| INDEX_LENGTH | bigint unsigned | BIGINT | 已实现 |
| DATA_FREE | bigint unsigned | BIGINT | 已实现 |
| AUTO_INCREMENT | bigint unsigned | BIGINT | 已实现 |
| CREATE_TIME | timestamp | timestamp | 已实现 |
| UPDATE_TIME | datetime | VARCHAR(0) | 未实现 |
| CHECK_TIME | datetime | VARCHAR(0) | 未实现 |
| TABLE_COLLATION | varchar(64) | VARCHAR(0) | 未实现 |
| CHECKSUM | bigint | BIGINT | 已实现 |
| CREATE_OPTIONS | varchar(256) | VARCHAR(0) | 未实现 |
| TABLE_COMMENT | text | VARCHAR(5000) | 已实现 |
