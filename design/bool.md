## MO bool数据类型说明
### 1、MO-DB bool数据类型简介
使用bool和boolean表示bool数据类型，数据类型bool包含两个不同的值，true和false。若不存在非空约束，布尔类型还支持unknown作为null值。
bool类型的列只支持接受以下类型的字符串来表示’true‘:
true（忽略大小写）
1
以及以下字符串表示'false'：
false（忽略大小写）
0

当对bool列插入除以上的其他值时，弹出输入的数值不对的错误提示。

注意，这里和mysql不同的是，这里的bool类型不看成为tinyint(1), 就是单独的bool类型，因此其只能参与逻辑运算，不应参与其他与整数或者浮点数有关的一些运算操作，比如加减乘除。

对于bool类型，输出时，只显示为true,false以及null。

### 2、语法介绍
```sql
create table t1(a bool, b boolean);

insert into t1 values(1, 0), (true, false);

select * from t1;
+-------+------+
|   a   |  b  |
+-------+------+
| true | false |
| true | false |
+-------+------+

show columns from t1;
+-------+----------------+------+------+---------+
| Field | Type | Null | Key  | Default | Extra   |
+-------+----------------+------+------+---------+
| a     | bool |      |      | NULL    |         |
| b     | bool |      |      | NULL    |         |
+-------+----------------+------+------+---------+

```


### 3、逻辑运算符
逻辑运算符主要用来判断表达式的真假，逻辑运算符返回的结果为1，0或者null。
MO中支持的四种逻辑运算符如下：
| 运 算 符 | 作  用 | 示  例 |
|:-:|:-:|:-:|
| NOT 或 ! | 逻辑非 | SELECT NOT A <br> SELECT !A |
| AND 或 &&| 逻辑与 | SELECT A AND B <br> SELECT A && B |
| OR 或 \|\| | 逻辑或 | SELECT A OR B <br> SELECT A \|\| B |
| XOR | 逻辑异或 | SELCT A XOR B |

逻辑非真值表
||NOT|
|:-:|:-:|
|True|False|
|False|True|
|Unknown|Unknown|

逻辑与真值表
|AND|True|False|Unknown|
|:-:|:-:|:-:|:-:|
|True|True|False|Unknown|
|False|False|False|False|
|Unknown|Unknown|False|Unknown|

逻辑或真值表
|OR|True|False|Unknown|
|:-:|:-:|:-:|:-:|
|True|True|True|True|
|False|True|False|Unknown|
|Unknown|True|Unknown|Unknown|

逻辑异或真值表
|XOR|True|False|Unknown|
|:-:|:-:|:-:|:-:|
|True|False|True|Unknown|
|False|True|False|Unknown|
|Unknown|Unknown|Unknown|Unknown|

而这些运算符的优先级如下:
| 优先级由低到高排列 | 运算符 |
|:-:|:-:|
| 1 | \|\|、 OR |
| 2 | XOR |
| 3 | &&、 AND |
| 4 | NOT |

注意：&&，||，! 这三个符号在MySQL中已被弃用，会在未来的版本中删除，MySQL建议使用AND、OR、NOT进行替换。 

### 4、条件运算符
支持的条件运算符如下：>、<、=、!=、<>、>=、<= 这7种。
```sql
create table t1(a int);
insert imto t1 values(1), (3);
select a < 2 from t1;
+-------+
|a < 10 |
+-------+
| true |
| false|
+-------+
```
条件运算符返回的结果为bool类型，因此可以与上述的逻辑运算符组合起来组成逻辑表达式，但注意bool类型不参与算术表达式的运算。(这里bool是否可以参与条件运算符呢？)

```sql
create table t1(a int, b int);
insert into t1 values(1, 2),(3, 4);
select a < 2 && b < 3 from t1;
+----------------+
| a < 2 && b < 3 |
+----------------+
|           true |
|          false |
+----------------+
```


### 5、聚合函数
对于bool类型列的聚合函数，这里只支持count()函数来对行数进行统计，其他的聚合函数不予实现支持。
|Name|Description|
|:-:|:-:|
|AVG()|不支持|
|COUNT()|支持|
|MAX()|不支持|
|MIN()|不支持|
|SUM()|不支持|

### 6、where子句
对于bool类型数据，由于是bool类型，因此可以将该列作为where子句中的查询条件。例如：
```sql
create table t1(a bool);
insert into t3 values(1), (0), (null);
select * from t3 where a;
+------+
| a    |
+------+
| true |
+------+
1 row in set (0.00 sec)
```

也可以组成其他更复杂一点的逻辑表达式来组成where子句的查询条件。
```sql
create table t1(a int, b int);
insert into t1 values(1, 2),(3, 4);
select * from t1;
+------+------+
| a    | b    |
+------+------+
|    1 |    2 |
|    3 |    4 |
+------+------+
2 rows in set (0.00 sec)

select * from t1 where a < 2 && b < 3;
+------+------+
| a    | b    |
+------+------+
|    1 |    2 |
+------+------+
1 row in set (0.01 sec)

select * from t1 where a < 2 && b < 3 && 0;
Empty set (0.00 sec)
```

### 7、bool值的排序
对于bool列的排序，按照true大于false的原则，null值升序放在false之前，降序时放在最后。
```sql
create table t1(a bool);
insert into t1 values(0),(1),(null);
select * from t1 order by a asc;
+------+
| a    |
+------+
| NULL |
| false|
| true |
+------+
3 rows in set (0.01 sec)

```

### 7、工作计划
|工作内容|工作时间安排|
|:-:|:-:|
|bool语法支持| 1天 |
|bool类型的CURD操作| 4天 |
|逻辑运算符| 3天 |
|条件运算符和where子句| 3天 |
