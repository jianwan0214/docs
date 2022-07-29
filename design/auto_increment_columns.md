## 自增列功能说明

### 1、基本概念
自增列（auto_increment）是用于自动填充缺省列值的列属性。当 INSERT 语句没有指定 auto_increment 列的具体值时，系统会自动地为该列分配一个值。目前mo只支持对int32，int64类型的列为自增列，一张表中不允许设置两个及以上自增列，，其sql语法如下：
```sql
## 设置int32类型的自增列
create table t(a int primary key auto_increment);

## 设置int64类型的自增列
create table t(a int64 primary key auto_increment);

create table t(a int64 primary key auto_increment, b int64 auto_increment);
```

### 2、自增列的功能
当insert语句没有给自增列指定值，或者指定的值为null时，系统就会为该列分配一个值，值为该列目前的最大值+1。
```sql
create table t(id int primary key auto_increment, c int);
insert into t(c) values (3), (4), (5);
select * from t;
+----+------+
| id | c    |
+----+------+
|  1 |    3 |
|  2 |    4 |
|  3 |    5 |
+----+------+

insert into t values(null,1);
select * from t;
+----+------+
| id | c    |
+----+------+
|  1 |    3 |
|  2 |    4 |
|  3 |    5 |
|  4 |    1 |
+----+------+
4 rows in set (0.00 sec)
```

此外，auto_increment还支持显式指定列值的插入语句，此时mo会保存显式指定的值：
```sql
insert into t values(6, 6);
select * from t;
+----+------+
| id | c    |
+----+------+
|  1 |    3 |
|  2 |    4 |
|  3 |    5 |
|  6 |    6 |
+----+------+

insert into t(c) values(7);
select * from t;
+----+------+
| id | c    |
+----+------+
|  1 |    3 |
|  2 |    4 |
|  3 |    5 |
|  6 |    6 |
|  7 |    7 |
+----+------+
```

当自增列数值超过该列的最大值时，则报溢出错误。
```sql
insert into t VALUES (2147483647, 6);
select * from t;
+------------+------+
| id         | c    |
+------------+------+
|          1 |    3 |
|          2 |    4 |
|          3 |    5 |
|          6 |    6 |
|          7 |    7 |
| 2147483647 |    6 |
+------------+------+

insert into t(c) values (6);
ERROR 1690 (22003): constant 2147483648 overflows int
```

## 3、实现原理
  首先在catalog上建立一张自增列的表，表名称为mo_increment_columns，每一行对应一个自增列，表定义为
```sql
show columns from mo_increment_columns;
+-------+---------+------+------+---------+---------+
| Field | Type    | Null | Key  | Default | Comment |
+-------+---------+------+------+---------+---------+
| name  | VARCHAR | YES  |      | NULL    |         |
| num   | BIGINT  | YES  |      | NULL    |         |
+-------+---------+------+------+---------+---------+
```
name列为dbname_tablename_colname， num列为该自增列所对应的当前最大值，初始化值为0。

当有自增列的表建立时，就会在mo_increment_columns表中插入一条记录，name列为dbname + "\_" + tablename + "\_" + colname， num值默认设置为0。  
当对有自增列的表进行插入操作时，在访问mo_increment_columns表获取num值时，需要启动一个事务去访问，来获取num值，当获取失败时，需要进行重试，直到获取num值成功。
### 3.1、insert语句中自增列没有值
 此时需要访问mo_increment_columns去获取num值，并且将mo_increment_columns表中的num+1，update到mo_increment_columns表中去，然后将取出的num+1设置到insert语句中去，进行后续的insert操作。
### 3.2、insert语句中自增列有特定值
  此时需要去设置mo_increment_columns的num值，启动一个事务去update num的值。当特定值小于该num值时，mo_increment_columns中的num不予更新。
### 3.3、支持批量的insert语句
   此时需要访问mo_increment_columns去获取num值，并且将mo_increment_columns表中的num+n，update到mo_increment_columns表中去，然后将取出的num+i设置到insert语句中去，进行后续的insert操作。
### 3.4、update自增列时需要更新num值
  当使用update语句更新自增列的值时，需要启动一个事务去update num的值，当更新的自增列的最大值大于mo_increment_columns的num值时，需要将num值进行更新，否则不需要更新。

注意：
当删除了有自增列表中的数据时，不会对mo_increment_columns的num值有影响，只有在insert和update操作后，才有可能影响mo_increment_columns中的num值。
自增列只能保证自增列的单调性，不保证严格的递增性。例如同时插入两条语句时，当先获取自增列的insert语句失败时，并不会对mo_increment_columns表中num值进行回滚，下一条insert语句会接着num值进行增加操作。

## 4、使用限制
\* 只能定义在类型为int32、int64的列上。  
