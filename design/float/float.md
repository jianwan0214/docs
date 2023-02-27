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
mysql> create table t1(a float(1, 0));
Query OK, 0 rows affected, 1 warning (0.03 sec)

mysql> insert into t1 values(1.4), (1.5), (1.6);
Query OK, 3 rows affected (0.01 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql> select * from t1;
+------+
| a    |
+------+
|    1 |
|    1 |
|    2 |
+------+
3 rows in set (0.00 sec)
```
可以看到在mysql的转换中，将1.5四舍五入为了1。
