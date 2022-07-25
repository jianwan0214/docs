## load from S3 功能说明
### 1、功能简介
 此功能即实现从S3中读取文件，实现load数据到本地数据库表中的功能。目前的S3支持AWS，以及国内的主流云厂商，此外S3上的文件支持压缩格式，不过目前只支持对S3上一个文件的读取进行load，不支持多个文件的读取。
 
### 2、语法介绍
```sql
LOAD DATA
    [LOW_PRIORITY | CONCURRENT] [LOCAL]
    INFILE
    s3option(endpoint, [aws_access_key_id, aws_secret_access_key],[bucket, filepath, region, [compression]])
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
|aws_access_key_id| S3的Access key ID|
|aws_secret_access_key| S3的Secret access key|
|bucket| 需要访问的桶|
|filepath| 访问文件的相对路径 |
|region| s3所在的区域|
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为""，"none"，"gzip"，"bzip2"，"flate"，"zlib"|

其他的字段解释可以参考 https://github.com/matrixorigin/docs/blob/main/notes/load_data_notes.txt

示例：
```sql
##非压缩文件格式
LOAD DATA INFILE s3option('s3.us-west-2.amazonaws.com', ['ABCD', 'ABCD'], ['wangjian-test', 'a.txt', 'us-west-2']) INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';

##压缩文件格式
LOAD DATA INFILE s3option('s3.us-west-2.amazonaws.com', ['ABCD', 'ABCD'], ['wangjian-test', 'a.txt.gz', 'us-west-2',['gzip']]) INTO TABLE t1 FIELDS TERMINATED BY ',' ENCLOSED BY '\"' LINES TERMINATED BY '\n';
```

### 3、功能限制
目前此功能只支持对一个文件的读取进行load操作，对于压缩或非压缩格式文件均只支持一个文件的load操作；另外对于压缩格式的文件，目前只支持如下的压缩格式：
|Field|Description|
|:-:|:-:|
|compression| S3文件的压缩格式，为空表示非压缩文件，支持的字段为""，"none"，"gzip"，"bzip2"，"flate"，"zlib"|
