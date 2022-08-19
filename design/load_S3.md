## load from S3 功能说明
### 1、功能简介
 此功能即实现从S3中读取文件，实现load数据到本地数据库表中的功能。目前的S3支持AWS，以及国内的主流云厂商，此外S3上的文件支持压缩格式，此外还支持文件路径的正则表达式规则，来读取多个文件，例如"/Users/*.txt"就会去读取以txt结尾的所有文件，其中文件的读取顺序不确定。
 
### 2、语法介绍
```sql
LOAD DATA
    [LOW_PRIORITY | CONCURRENT] [LOCAL]
    [ INFILE 'string'
    | INFILE {"filepath"='<string>', "compression"='<string>'}
    | URL s3options {"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'}
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
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为"auto", "none", "gzip", "bzip2", "flate", "zlib", "lz4"|
对于压缩格式，"auto"表示通过文件后缀名自动检查文件的压缩格式，"none"表示为非压缩格式，其余表示文件的压缩格式。

其他的字段解释可以参考 https://github.com/matrixorigin/docs/blob/main/notes/load_data_notes.txt

这里的执行会将load改写成insert into select plan进行执行，但对于客户端不感知，改写后的逻辑和如下sql语句相同：
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
## 本地文件load
LOAD DATA INFILE 'a.txt' INTO TABLE t1 FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

## 本地多个文件load
LOAD DATA INFILE '/Users/*.txt' INTO TABLE t1 FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

## 本地文件指定压缩格式load
LOAD DATA INFILE {'filepath'='a.txt',"commpression"="none"} INTO TABLE t1 FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

## 本地文件自动检查压缩格式load
LOAD DATA INFILE {'filepath'='a.txt',"commpression"="auto"} INTO TABLE t1 FIELDS TERMINATED BY '|' ENCLOSED BY '\"' LINES TERMINATED BY '\n' IGNORE 1 LINES;

## 非指定文件压缩格式
LOAD DATA INFILE URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>'} INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

## 指定文件压缩格式
LOAD DATA INFILE URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'} INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

```

### 3、功能限制
对于压缩格式的文件，目前只支持如下的压缩格式：
|Field|Description|
|:-:|:-:|
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为"auto", "none"，"gzip"，"bzip2"，"flate"，"zlib", "lz4"|

### 4、外表
这里需要将load操作转换成insert into t1 select * from t2的形式，其中t2就是外表，t1是要插入的表，一般为普通表，这里介绍一下外表的概念。这里暂时还不支持往外表中插入数据。
外表语句语法：
```sql
## 创建指向本地文件的外表（指定压缩格式）
create external table t(...) localfile{"filepath"='<string>', "compression"='<string>'} FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

## 创建指向本地文件的外表（不指定压缩格式，则为auto格式，自动检查文件的格式）
create external table t(...) localfile{"filepath"='<string>'} FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';


## 创建指向S3文件的外表（指定压缩格式）
create external table t(...) URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>', "compression"='<string>'} FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

## 创建指向S3文件的外表（不指定压缩格式，则为auto格式，自动检查文件的格式）
create external table t(...) URL s3option{"endpoint"='<string>', "access_key_id"='<string>', "secret_access_key"='<string>', "bucket"='<string>', "filepath"='<string>', "region"='<string>'} FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';
```

其中，外表目前只支持进行select操作，其余的delete，insert, update目前来说，都不予支持。select的操作与目前普通表的操作相同，支持where, limit等条件操作。
创建外表后，可以使用select操作读取外表指向文件里的内容。在往现有普通表中导入数据时，也可以采取如下的操作
```sql
## t1为普通表，t为外表
insert into t1 select * from t;

## 也可以选取文件中某一列
insert into t1 select a from t;
```

外表在catalog中的存储形式：
mo_tables表：

|relname|reldatabase|relpersistence|relkind|rel_comment|rel_createsql|
|:-:|:-:|:-:|:-:|:-:|:-:|
|t|ssb|p|e|| {"Fields":null,"Lines":null,"IgnoredLines":0,"ColumnList":null,"Assignments":null,"Filepath":"/Users/wangjian/test/a.txt","Config":{"Endpoint":"","Bucket":"","KeyPrefix":""},"LoadType":0,"CompressType":"auto","S3options":null}|

对于外表，relkind设置为"e"，此类型表示该表为外部表，rel_createsql字段中以字符串形式存放与外表相关的一些参数，以用于后续的读取文件。


