## MO 数据库 load 性能分析
### 1、外表大文件的拆分性能
对于50G的文件，首先创建了指向该文件的外表t，然后使用导出命令中的max_file_size字段，将每个文件最大值设置为1G，具体与如下：
```sql
  select * from t into outfile 'a.txt' max_file_size 1048576; // 文件最大值单位为KB
```
生成的50多个文件名称为，a.txt, ..., a.txt.49。 其中在本人的电脑上，耗时为13min49秒。（本人电脑内存为16G，芯片为Apple M1 Pro, 主频3.35 GHz）。
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/export.png)

![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/file.png)

### 2、对于1G以上数据的性能分析
对于load操作是改写成insert into t1 select * from t2; 因此在这里也将select普通表放入进行对比。
首先是对于1G的数据，首先来对比设置 DefaultTxnCacheSize 大小后带来的性能改善：
|DefaultTxnCacheSize| insert into t2 select from t1(普通表) | insert into t1 select from t(外表)(load) | load事务提交耗时 |
|:-:|:-:|:-:| :-:|
| 256 M | 29.29 sec | 28.16 sec| 10 sec |
| common.UNLIMIT | 10.30 sec | 16.95 sec | 8 sec |

其中 DefaultTxnCacheSize = 256 M 时的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_1G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph.png)

DefaultTxnCacheSize = 256 M 时的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_1G_fullMem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph.png)


其中 DefaultTxnCacheSize = common.UNLIMIT (UINT64_MAX) 时的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_1G_fullMem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_fullMem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph_fullMem.png)

DefaultTxnCacheSize = common.UNLIMIT (UINT64_MAX) 时的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_insert_1G_fullMem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_fullMem.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph_fullMem.png.png)
