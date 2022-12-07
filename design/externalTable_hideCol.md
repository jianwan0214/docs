## 外表的隐藏列设计

# 1、外表的隐藏列说明

目前MO中对于外表新增如下的隐藏列（目前只支持表中所列的隐藏列，后续如有新增，持续更新文档）

| 隐藏列列名 | 隐藏列类型 | 隐藏列含义 |
|:-:|:-:|:-:|
| __mo_filepath | varchar | 数据行所在的文件名 |

例如创建了一个外表，可以查看列定义，select * 选择数据，均不会出现隐藏列的信息。
```
>>> create external table t1(a int, b int, c int) infile{"filepath"="/Users/wangjian/test/*.txt"};

>>> show columns from t1;
+-------+------+------+------+---------+-------+---------+
| Field | Type | Null | Key  | Default | Extra | Comment |
+-------+------+------+------+---------+-------+---------+
| a     | INT  | YES  |      | NULL    |       |         |
| b     | INT  | YES  |      | NULL    |       |         |
| c     | INT  | YES  |      | NULL    |       |         |
+-------+------+------+------+---------+-------+---------+
3 rows in set (0.02 sec)

>>> select * from t1;
+------+------+------+
| a    | b    | c    |
+------+------+------+
|   10 |    2 |    3 |
|   20 |    3 |    4 |
|   40 |    5 |    6 |
|    1 |    2 |    3 |
|    2 |    3 |    4 |
|    4 |    5 |    6 |
+------+------+------+
6 rows in set (0.05 sec) 

```

而显式 select 该隐藏列则可以查询到对应的信息
```
>>> select __mo_filepath from t1;
+----------------------------+
| __mo_filepath              |
+----------------------------+
| /Users/wangjian/test/b.txt |
| /Users/wangjian/test/b.txt |
| /Users/wangjian/test/b.txt |
| /Users/wangjian/test/a.txt |
| /Users/wangjian/test/a.txt |
| /Users/wangjian/test/a.txt |
+----------------------------+
6 rows in set (0.00 sec)

>>> select *, __mo_filepath from t1;
+------+------+------+----------------------------+
| a    | b    | c    | __mo_filepath              |
+------+------+------+----------------------------+
|   10 |    2 |    3 | /Users/wangjian/test/b.txt |
|   20 |    3 |    4 | /Users/wangjian/test/b.txt |
|   40 |    5 |    6 | /Users/wangjian/test/b.txt |
|    1 |    2 |    3 | /Users/wangjian/test/a.txt |
|    2 |    3 |    4 | /Users/wangjian/test/a.txt |
|    4 |    5 |    6 | /Users/wangjian/test/a.txt |
+------+------+------+----------------------------+
6 rows in set (0.01 sec)

```

# 2、外表隐藏列的使用
对于外表新增的隐藏列，是用来在where条件中加上过滤进行扫描加速的，这样就不需要对不符合条件的文件进行扫描，从而加快select速度，其操作过程如下：
```
 >>> select *,__mo_filepath from t1 where __mo_filepath = '/Users/wangjian/test/b.txt';
+------+------+------+----------------------------+
| a    | b    | c    | __mo_filepath              |
+------+------+------+----------------------------+
|   10 |    2 |    3 | /Users/wangjian/test/b.txt |
|   20 |    3 |    4 | /Users/wangjian/test/b.txt |
|   40 |    5 |    6 | /Users/wangjian/test/b.txt |
+------+------+------+----------------------------+
3 rows in set (0.02 sec)

>>> select *,__mo_filepath from t1 where __mo_filepath in ('/Users/wangjian/test/b.txt');
+------+------+------+----------------------------+
| a    | b    | c    | __mo_filepath              |
+------+------+------+----------------------------+
|   10 |    2 |    3 | /Users/wangjian/test/b.txt |
|   20 |    3 |    4 | /Users/wangjian/test/b.txt |
|   40 |    5 |    6 | /Users/wangjian/test/b.txt |
+------+------+------+----------------------------+
3 rows in set (0.01 sec)

>>> select *,__mo_filepath from t1 where __mo_filepath like '%b.txt';
+------+------+------+----------------------------+
| a    | b    | c    | __mo_filepath              |
+------+------+------+----------------------------+
|   10 |    2 |    3 | /Users/wangjian/test/b.txt |
|   20 |    3 |    4 | /Users/wangjian/test/b.txt |
|   40 |    5 |    6 | /Users/wangjian/test/b.txt |
+------+------+------+----------------------------+
3 rows in set (0.04 sec)

>>> select *,__mo_filepath from t1 where __mo_filepath = '/Users/wangjian/test/b.txt' or __mo_filepath = '/Users/wangjian/test/a.txt';
+------+------+------+----------------------------+
| a    | b    | c    | __mo_filepath              |
+------+------+------+----------------------------+
|    1 |    2 |    3 | /Users/wangjian/test/a.txt |
|    2 |    3 |    4 | /Users/wangjian/test/a.txt |
|    4 |    5 |    6 | /Users/wangjian/test/a.txt |
|   10 |    2 |    3 | /Users/wangjian/test/b.txt |
|   20 |    3 |    4 | /Users/wangjian/test/b.txt |
|   40 |    5 |    6 | /Users/wangjian/test/b.txt |
+------+------+------+----------------------------+
6 rows in set (0.06 sec)
```
