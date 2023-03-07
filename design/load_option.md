## MO load功能说明
此文档对load操作中相关参数进行示例和说明。

LOAD DATA INFILE '/Users/wangjian/test/a.txt' INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n' ignore 1 lines/rows;
### 1、分隔符说明
MO 的load工具使用的是simdcsv，具体的代码位置可以参见 https://github.com/matrixorigin/simdcsv。
load语句中可以通过 FIELDS TERMINATED BY 表示字段分割符。通常是','。而ssb的是'|'。

### 2、包括符说明
通过 FIELDS ENCLOSED BY 'char'来设置，simdCSV固定是'"'（英文双引号）。当不设置该字段值，默认即为'"', 当列数据内部存在分隔符时，需要使用双引号将该字段包围起来。  
例如当分隔符为','，包括符为单引号'''(单引号)， 当字段为内容为 'abc,acb' 此时就需要将该字段使用双引号括起来，为 "'abc,acb'" 
当字段内容中也存在双引号和分隔符同时在一起时，即形如 'abc",abc' 时，就需要在双引号前方加上转义字符'"'(双引号)，结果即为 "'abc"",abc'", 此时才会成功的解析出字段内容
abc",abc

### 3、结束符说明
通过LINES TERMINATED BY 'char'来设置，即为一行的结束符号，常见的有'\n', '\r\n'

### 4、rfc4180标准介绍
rfc4180是一种关于csv文件的格式，其中规定的格式如下：  
1、每条记录位于单独的一行，由换行符（CRLF）分隔，例如：  
aaa,bbb,ccc CRLF  
zzz,yyy,xxx CRLF
