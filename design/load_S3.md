## load from S3 功能说明
### 1、功能简介
 此功能即实现从S3中读取文件，实现load数据到本地数据库表中的功能。目前的S3支持AWS，以及国内的主流云厂商，此外S3上的文件支持压缩格式，不过目前只支持对S3上一个文件的读取进行load，不支持多个文件的读取。
 
### 2、语法介绍
```sql
LOAD DATA
    [LOW_PRIORITY | CONCURRENT] [LOCAL]
    INFILE
    URL s3options {"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'}
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [PARTITION (partition_name [, partition_name] ...)]
    [CHARACTER SET charset_name]
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number {LINES | ROWS}]
    [(col_name_or_user_var
        [, col_name_or_user_var] ...)]
    [SET col_name={expr | DEFAULT}
        [, col_name={expr | DEFAULT}] ...]
```
其中
|Field|Description|
|:-:|:-:|
|endpoint|终端节点是作为 AWS Web 服务的入口点的 URL。例如：s3.us-west-2.amazonaws.com|
|access_key_id| S3的Access key ID|
|secret_access_key| S3的Secret access key|
|bucket| 需要访问的桶|
|filepath| 访问文件的相对路径 |
|region| s3所在的区域|
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为"none"，"gzip"，"bzip2"，"flate"，"zlib"|

其他的字段解释可以参考 https://github.com/matrixorigin/docs/blob/main/notes/load_data_notes.txt

这里要将load改写成insert into select语句，改写后的sql语句如下：
```sql
insert into [<namespace>.]<table_name> FROM { internalStage | externalStage }
    [PARTITION (partition_name [, partition_name] ...)]
    [CHARACTER SET charset_name]
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number {LINES | ROWS}]
    [(col_name_or_user_var
        [, col_name_or_user_var] ...)]
    [SET col_name={expr | DEFAULT}
        [, col_name={expr | DEFAULT}] ...]

其中：
internalStage ::= 
{"filepath"='<string>'}
| {"filepath"='<string>', "compression"='<string>'}

externalStage ::=
URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>'}
| URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>' "compression"='<string>'}
```


示例：
```sql
##本地文件load
LOAD DATA INFILE 'a.txt' INTO TABLE t1 FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

##改写后语句
insert into t1 from {"filepath"='a.txt'} FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;


##非指定文件压缩格式
LOAD DATA INFILE URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>'} INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

##改写后语句
insert into t1 from URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>'} FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

##指定文件压缩格式
LOAD DATA INFILE URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'} INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

##改写后语句
insert into t1 from URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'} FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;
```

### 3、功能限制
目前此功能只支持对一个文件的读取进行load操作，对于压缩或非压缩格式文件均只支持一个文件的load操作；另外对于压缩格式的文件，目前只支持如下的压缩格式：
|Field|Description|
|:-:|:-:|
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为"none"，"gzip"，"bzip2"，"flate"，"zlib"|
