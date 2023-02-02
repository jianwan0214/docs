## 外表的并行load

### 1、并行load的实现细节

对于一个格式良好的大文件，例如jsonline文件或者一行数据中没有换行符的csv等普通文件，就可以对该文件进行并行load，以加快load的性能。
例如，对于2个G的大文件，使用两个线程去进行load，第2个线程先seek到1G的位置，然后一直往后读，直到找到换行符，然后从换行符后开始进行文件读取，
进行load。这样就可以做到两个线程，每个线程读取1G的数据。

对于csv等普通文件，需要在load语句最后面增加一个字段 PARALLEL 来开启并行load这个优化操作，具体语句如下：
 ```
 // 打开并行load优化
load data infile 'XXX' into table XXX PARALLEL 'TRUE';

 // 关闭并行load优化
load data infile 'XXX' into table XXX PARALLEL 'false';

 // 默认关闭并行load优化
load data infile 'XXX' into table XXX;
```
其中 PARALLEL 后面加true表示load的文件中一行内容不存在换行符，可以进行并行load操作。如果不加此字段，则默认为false。
对于jsonline文件，因为文件中的一行内容不会存在换行符，因此jsonline文件的load语句默认开启并行load操作。


### 2、并行load的实现方案
对于要导入的 m 个文件，通常会根据机器的cpu个数生成对应的线程数，假设为 n 个，此时就需要对每个线程分配对应的处理文件。
对于设置了 PARALLEL 为true的情况，就需要对大文件进行并行读以加快性能。
对于多线程读大文件，目前考虑的方案有：  
1、对于 m 个文件，n 个线程，每个线程都平均的去读每个文件，即对于每个文件，都用 n 个线程去并行的读，达到最大的并发度。

![Image](https://github.com/jianwan0214/docs/blob/main/design/load/WechatIMG80.png)   

其中offset表示seek的位置，size表示要读的字节数，-1则表示读到文件结束。
此方案则不考虑具体的文件大小，对每个文件拆分成 n 等分的位置，每个线程去读对应的文件内容。

2、对于 m 个文件，n 个线程，考虑对小文件不进行拆分，仅对大文件进行拆分，并且尽可能的均分，使得每个线程需要读取的线程数相差不大。   
这里的大文件的标准可以设置为超过1G就对其进行拆分read。

对于每个线程自己所对应的文件的部分，先seek到文件的指定位置，通常为文件大小/线程个数* 对应线程号，然后再读出第一个换行符'\n'，从而得到准确的offset。

### 3、并行load的限制
1、开启并行load操作时必须要保证文件中每行数据中不包含指定的行结束符，比如'\n'，否则会导致文件load数据出错。
2、文件的并行load要求文件必须是非压缩格式，压缩格式的文件不支持并行load。
