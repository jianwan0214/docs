## load性能分析报告
与0.5.0性能比较

改为分批4w行数据commit后的性能：

|FileSize|  0.5.0 load性能 | load(分批commit) | load(整体commit) | load事务提交耗时 |
|:-:|:-:|:-:| :-:| :-:|
| 1 G |  5.49 sec  | 8.84 sec | 16.95 sec | 8 sec |
| 2 G |  14.85 sec | 20.02 sec | 35.10 sec | 17 sec |
| 3 G |  24.81 sec | 32.26 sec | 1 min 3.68 sec | 35 sec |
| 4 G |  39.92 sec | 41.14 sec | 1 min 48.65 sec | 68 sec |
| 5 G |  1 min 10.42 sec  | 1 min 4.04 sec | 2 min 55.56 sec | 107 sec |

各数据量下性能数据分析：
### 1G数据
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/1G_load_MEM.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/1G_load_MEM_detail.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/1G_load_time.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/1G_load_time_detail.png)

### 2G数据
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/2G_load_MEM.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/2G_load_MEM_detail.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/2G_load_time.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/2G_load_time_detail.png)

### 3G数据
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/3G_load_MEM.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/3G_load_MEM_detail.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/3G_load_time.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/3G_load_time_detail.png)

### 4G数据
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/4G_load_MEM.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/4G_load_MEM_detail.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/4G_load_time.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/4G_load_time_detail.png)

### 5G数据
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/5G_load_MEM.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/5G_load_MEM_detail.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/5G_load_time.png)
![Image](https://github.com/jianwan0214/docs/blob/main/design/preformance_2/5G_load_time_detail.png)
