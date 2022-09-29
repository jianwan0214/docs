## load的性能分析
### 1、外表大文件的拆分性能
对于50G的文件，首先创建了指向该文件的外表t，然后使用导出命令中的max_file_size字段，将每个文件最大值设置为1G，具体与如下：
```sql
  select * from t into outfile 'a.txt' max_file_size 1048576; // 文件最大值单位为KB
```
生成的50多个文件名称为，a.txt, ..., a.txt.49。 其中在本人的电脑上，耗时为

![Image](https://raw.githubusercontent.com/Gladysid/Images-blog/master/IE-box-pic.png)
