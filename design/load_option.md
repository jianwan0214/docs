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

2、文件中最后一条记录可能有也可能没有结束换行符。例如：  
aaa,bbb,ccc CRLF  
zzz,yyy,xxx

3、可能有一个可选的标题行作为文件的第一行出现，其格式与普通记录行相同。此标题将包含与文件中的字段相对应的名称，并且应包含与文件其余部分中的记录相同数量的字段。例如：  
field_name,field_name,field_name CRLF  
aaa,bbb,ccc CRLF  
zzz,yyy,xxx CRLF

4、在标题和每条记录中，可能有一个或多个字段，以逗号分隔。在整个文件中，每一行都应包含相同空格被视为字段的一部分，不应忽略。记录中的最后一个字段后面不能跟逗号。例如：  
aaa,bbb,ccc

5、每个字段可以用双引号括起来，也可以不用双引号括起来。如果字段没有用双引号引起来，那么双引号不能出现在字段内。例如：  
"aaa","bbb","ccc" CRLF  
zzz,yyy,xxx 

6、包含换行符（CRLF）、双引号和逗号的字段应该用双引号引起来。例如：  
"aaa"，"b CRLF  
bb"，"ccc" CRLF  
zzz,yyy,xxx 

7、如果使用双引号将字段括起来，那么出现在字段内的双引号必须通过在其前面加上另一个双引号。例如：  
"aaa","b""bb","ccc"  

### 5、MO load文件相关设置的说明
