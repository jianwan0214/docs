## MO 数据库 load 性能分析
### 1、外表大文件的拆分性能
对于50G的文件，首先创建了指向该文件的外表t，然后使用导出命令中的max_file_size字段，将每个文件最大值设置为1G，具体与如下：
```sql
  select * from t into outfile 'a.txt' max_file_size 1048576; // 文件最大值单位为KB
```
生成的50多个文件名称为，a.txt, ..., a.txt.49。 其中在本人的电脑上，耗时为13min49秒。（本人电脑内存为16G，芯片为Apple M1 Pro, 主频3.35 GHz）。
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/export.png)

![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/file.png)

### 2、DefaultTxnCacheSize 大小对数据的性能影响
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

### 3、DefaultTxnCacheSize设为最大值情况下各数据集的性能分析
|FileSize| insert into t2 select from t1(普通表) | insert into t1 select from t(外表)(load) | load事务提交耗时 |
|:-:|:-:|:-:| :-:|
| 1 G | 10.30 sec | 16.95 sec | 8 sec |
| 2 G | 37.28 sec | 35.10 sec | 17 sec |
| 3 G | 58.25 sec | 1 min 3.68 sec | 35 sec |
| 4 G | 1 min 46.19 sec | 1 min 48.65 sec | 68 sec |
| 5 G | 4 min 36.75 sec | 2 min 55.56 sec | 107 sec |
（5G时insert into t2 select * from t1 事务提交耗时 3min）

改为分批4w行数据commit后的性能：
|FileSize| load(分批commit) | load(整体commit) | load事务提交耗时 |
|:-:|:-:|:-:| :-:|
| 1 G | 8.84 sec | 16.95 sec | 8 sec |
| 2 G | 20.02 sec | 35.10 sec | 17 sec |
| 3 G | 32.26 sec | 1 min 3.68 sec | 35 sec |
| 4 G | 41.14 sec | 1 min 48.65 sec | 68 sec |
| 5 G | 1 min 4.04 sec | 2 min 55.56 sec | 107 sec |

load(分批commit) batch
|FileSize| 2w | 4w | 6w | 8w | 10w |
|:-:|:-:|:-:| :-:|:-:| :-:|
| 1 G | 8.95 ses | 9.10 sec | 9.07 sec | 8.97 sec | 9.12 sec |
| 2 G | 17.75 ses | 18.02 sec | 17.98 sec | 18.78 sec | 18.79 sec |
| 3 G | 28.26 ses | 28.33 sec | 29.03 sec | 28.35 sec | 28.42 sec |
| 4 G | 40.03 ses | 40.06 sec | 39.70 sec | 41.64 sec | 41.52 sec |
| 5 G | 51.59 ses | 53.13 sec | 53.71 sec | 51.88 sec | 53.80 sec |


初步结论：  
目前影响load性能的两个关键点为：  
1、事务机制下insert不支持并发操作  
2、大规模数据的事务提交非常耗时。  

2G数据的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_2G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_2G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph_2G.png.png)

2G数据的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_insert_2G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_2G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph_2G.png)

3G数据的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_3G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_3G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph_3G.png)

3G数据的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_insert_3G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_3G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph_3G.png)

4G数据的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_4G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_4G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph_4G.png)

4G数据的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_insert_4G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_4G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph_4G.png)

5G数据的load相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_load_5G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_5G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/load_mem_graph_5G.png)

5G数据的 insert into t2 select * from t1 相关指标如下：
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/explain_insert_5G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_5G.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/performance/insert_mem_graph_5G.png)
