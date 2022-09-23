## MO show table status功能说明
    show table status功能和show tables功能类似，提供了该DB中所有非临时表的相关信息，可以显示指定要展示的db，也可以通过where子句对结果进行筛选。
### 1、show table status语句
    支持的相关语句如下:
    show table status;
    show table status from db;
    show table status where condition;
    
### 2、展示结果
```sql
mysql> show table status;
+------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
| Name | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment | Create_time         | Update_time         | Check_time | Collation          | Checksum | Create_options | Comment |
+------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
| t1   | InnoDB |      10 | Dynamic    |    1 |          16384 |       16384 |               0 |            0 |         0 |           NULL | 2022-09-20 16:44:10 | 2022-09-20 16:44:17 | NULL       | utf8mb4_0900_ai_ci |     NULL |                |         |
| t2   | InnoDB |      10 | Dynamic    |    1 |          16384 |       16384 |               0 |            0 |         0 |           NULL | 2022-09-20 16:44:23 | 2022-09-20 16:44:27 | NULL       | utf8mb4_0900_ai_ci |     NULL |                |         |
+------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
2 rows in set (0.05 sec)
```
上面展示了mysql中的结果，其中每个字段的含义可以参考mysql的文档：https://dev.mysql.com/doc/refman/8.0/en/show-table-status.html

其中MO所能支持的结果如下表给出：
|       展示列     | MySQL支持 | MO是否支持 |
|       :-:       |    :-:   |    :-:    |
|       Name      |     是    |     $\color{#FF0000}{是}$     |
|      Engine     |     是    |     否    |
|      Version    |     是    |     否    |
|    Row_format   |     是    |     否    |
|       Rows      |     是    |     否    |
|  Avg_row_length |     是    |     否    |
|   Data_length   |     是    |     否    |
| Max_data_length |     是    |     否    |
|   Index_length  |     是    |     否    |
|    Data_free    |     是    |     否    |
|  Auto_increment |     是    |     是    |
|   Create_time   |     是    |     是    |
|   Update_time   |     是    |     否    |
|   Check_time    |     是    |     否    |
|    Collation    |     是    |     否    |
|    Checksum     |     是    |     否    |
|  Create_options |     是    |     否    |
|     Comment     |     是    |     是    |
