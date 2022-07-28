## 自增列功能说明

### 1、基本概念
自增列（AUTO_INCREMENT）是用于自动填充缺省列值的列属性。当 INSERT 语句没有指定 AUTO_INCREMENT 列的具体值时，系统会自动地为该列分配一个值。目前mo只支持对int32，int64类型的列为自增列，并且该自增列必须设置为主键，且一张表中不允许设置两个自增列，只允许有一列自增列，其sql语法如下：
```sql
## 设置int32类型的自增列
create table t(a int primary key auto_increment);

## 设置int64类型的自增列
create table t(a int64 primary key auto_increment);

## 自增列未设置为主键时报错
create table t(a int auto_increment);
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key

## 设置两个以上自增列时报错
create table t(a int auto_increment, b int auto_increment, primary key(a, b));
ERROR 1075 (42000): Incorrect table definition; there can be only one auto column and it must be defined as a key
```

### 2、自增列的功能
当insert语句没有给自增列指定值，或者指定的值为null时，系统就会为该列分配一个值，值为该列目前的最大值+1，当该数值超过该列的最大值时，则报溢出错误。
```sql
insert into t2 values(null,1);
ERROR 1690 (22003): constant 2147483648 overflows int
```
