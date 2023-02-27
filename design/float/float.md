## 带精度浮点的MO实现方案

### 带精度浮点数的基本概念
  带精度的浮点数的基本语法为float(M, D), double(M, D)，其中mysql规定中，M的范围为（1=< M <=255），D的取值范围为（1=< D <=30），并且需要 M >= D，其中float和double的M，D的取值范围是一样的。这里建议 MO 的取值范围与mysql一致。下面为在mysql中执行的结果，并且注意到，在mysql中提示，带精度的浮点数类型会在未来的版本中移除。
```
mysql> create table t1(a float(1,0));
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> show warnings;
+---------+------+------------------------------------------------------------------------------------------------------------------+
| Level   | Code | Message                                                                                                          |
+---------+------+------------------------------------------------------------------------------------------------------------------+
| Warning | 1681 | Specifying number of digits for floating point data types is deprecated and will be removed in a future release. |
+---------+------+------------------------------------------------------------------------------------------------------------------+
1 row in set (0.02 sec)
```
在结果展示中，带精度的浮点数只会展示出要求的精度的位数，另外在位数不足时，会进行末尾补0操作。
```
mysql> create table t1(a float(5, 2));
Query OK, 0 rows affected, 1 warning (0.11 sec)

mysql> insert into t1 values(1), (2.5), (3.56), (4.678);
Query OK, 4 rows affected (0.07 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from t1;
+------+
| a    |
+------+
| 1.00 |
| 2.50 |
| 3.56 |
| 4.68 |
+------+
4 rows in set (0.00 sec)
```

### 带精度浮点数的实现细节
  mysql中对于精度浮点数的实现，是采取对输入的浮点数进行对应位数的四舍五入，具体的mysql文件参见 sql/field.cc中type_conversion_status Field_float::store(double nr)函数，以及Field_real::Truncate_result Field_real::truncate(double *nr, double max_value) 函数。具体代码逻辑如下：
```
/**
  If a field has fixed length, truncate the double argument pointed to by 'nr' appropriately.
  Also ensure that the argument is within [min_value; max_value] where
  min_value == 0 if unsigned_flag is set, else -max_value.

  @param[in,out] nr         the real number (FLOAT or DOUBLE) to be truncated
  @param[in]     max_value  the maximum (absolute) value of the real type

  @returns truncation result
*/

Field_real::Truncate_result Field_real::truncate(double *nr, double max_value) {
  if (std::isnan(*nr)) {
    *nr = 0;
    set_null();
    set_warning(Sql_condition::SL_WARNING, ER_WARN_DATA_OUT_OF_RANGE, 1);
    return TR_POSITIVE_OVERFLOW;
  } else if (is_unsigned() && *nr < 0) {
    *nr = 0;
    set_warning(Sql_condition::SL_WARNING, ER_WARN_DATA_OUT_OF_RANGE, 1);
    return TR_NEGATIVE_OVERFLOW;
  }

  if (!not_fixed) {
    double orig_max_value = max_value;
    uint order = field_length - dec;
    uint step = array_elements(log_10) - 1;
    max_value = 1.0;
    for (; order > step; order -= step) max_value *= log_10[step];
    max_value *= log_10[order];
    max_value -= 1.0 / log_10[dec];
    max_value = std::min(max_value, orig_max_value);

    /* Check for infinity so we don't get NaN in calculations */
    if (!std::isinf(*nr)) {
      double tmp = rint((*nr - floor(*nr)) * log_10[dec]) / log_10[dec];
      *nr = floor(*nr) + tmp;
    }
  }

  if (*nr < -max_value) {
    *nr = -max_value;
    set_warning(Sql_condition::SL_WARNING, ER_WARN_DATA_OUT_OF_RANGE, 1);
    return TR_NEGATIVE_OVERFLOW;
  } else if (*nr > max_value) {
    *nr = max_value;
    set_warning(Sql_condition::SL_WARNING, ER_WARN_DATA_OUT_OF_RANGE, 1);
    return TR_POSITIVE_OVERFLOW;
  }

  return TR_OK;
}
```

并且在mysql中，精度浮点数在经过四舍五入规范化之后，数值仍然保存为普通浮点数，因此可能会存在一些精度损失问题。例如：
```
mysql> create table t1(a float(5, 2));
Query OK, 0 rows affected, 1 warning (0.08 sec)

mysql> insert into t1 values(1.554), (1.555), (1.556);
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select * from t1;
+------+
| a    |
+------+
| 1.55 |
| 1.55 |
| 1.56 |
+------+
3 rows in set (0.00 sec)

mysql> create table t2(a decimal(30, 20));
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t2 select * from t1;
Query OK, 4 rows affected (0.03 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql> select * from t2;
+------------------------+
| a                      |
+------------------------+
| 1.54999995231628420000 |
| 1.54999995231628420000 |
| 1.55999994277954100000 |
+------------------------+
3 rows in set (0.00 sec)
```
可以看到在mysql的转换中，将1.555四舍五入为了1.55。这是因为1.555在float中具体位数为1.5549999475479125976562500，而在double类型中具体位数为1.5549999999999999378275106，
可以看到在进行四舍五入时，第三位小数位置为4，因此虽然字面值为1.555，但仍然会被舍入为1.55，这是由于浮点数的设计所造成的精度损失。因为浮点值是近似值而不是存储为精确值，所以尝试在比较中将它们视为精确值可能会导致问题，它们还受平台或实现依赖性的影响。


### 带精度浮点数的比较问题
  在mysql中，对于普通浮点数和带精度的浮点数比较，其结果会有一点不同。
```
mysql> create table t1(a float(5, 2));
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> insert into t1 values(90.012);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t1;
+-------+
| a     |
+-------+
| 90.01 |
+-------+
1 row in set (0.00 sec)

mysql> select * from t1 where a>90.01;
Empty set (0.01 sec)

mysql> create table t2(a float);
Query OK, 0 rows affected (0.03 sec)

mysql> insert into t2 values(90.01);
Query OK, 1 row affected (0.02 sec)

mysql> select * from t2 where a>90.01;
+-------+
| a     |
+-------+
| 90.01 |
+-------+
1 row in set (0.00 sec)

mysql> create table t3(a double);
Query OK, 0 rows affected (0.01 sec)

mysql> insert into t3 values(90.01);
Query OK, 1 row affected (0.06 sec)

mysql> select * from t3 where a>90.01;
Empty set (0.00 sec)
```
可以看到，90.01在普通float类型下，进行大于90.01的比较，左边为float类型，右边为double类型，在将float转成double类型中，存在精度损失，导致结果为true，这也是由于float的近似值表示所导致的。而double类型由于位数比较多，因此可以得到正确结果。
而对于带精度的float值比较，由于存在精度的控制，因此也能得到正确的结果。

### MO的带精度浮点数的0.8设计方案
  对于MO的精度浮点数设计，其M和D的取值范围与mysql一致，即（1=< M <=255），D的取值范围为（1=< D <=30），并且需要 M >= D，另外在结果展示中，考虑位数末尾补0操作。  
  精度浮点数的规范化操作与mysql一致，对指定位数进行四舍五入操作。将规范化后的数值依然保存为对应的float&double类型。
