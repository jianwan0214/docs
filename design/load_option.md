## MO load功能说明
此文档对load操作中相关参数进行示例和说明。

LOAD DATA INFILE '/Users/wangjian/test/a.txt' INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n' ignore 1 lines/rows;
### 1、分隔符说明
MO 的load工具使用的是simdcsv，具体的代码位置可以参见 https://github.com/matrixorigin/simdcsv。
load语句中可以通过 FIELDS TERMINATED BY 表示字段分割符。通常是','。而ssb的是'|'。

### 2、包括符说明
通过 FIELDS ENCLOSED BY 'char'来设置，simdCSV固定是'"'（英文双引号）。当不设置该字段值，默认即为'"', 当列数据内部存在分隔符时，需要使用双引号将该字段包围起来。
例如当分隔符为',',包括符为单引号‘‘’， 当字段为内容为 'abc,acb' 此时就需要将该字段使用双引号括起来，为 "'abc,acb'" 


### 3、结束符说明
