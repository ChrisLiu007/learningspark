# RDD分区的作用和原则
## 作用
1. 分区的第一个作用增加程序的并行度实现分布式的计算
2.  减少通信开销
## 原则
分区个数=集群中** CPU核心数 **, a partition is one thread.
# 设置分区的方法
1. 默认hashpartition 
2. 自定义分区方法Partitioner只支持元素类型为** 键值对 **的分区操作
*******
# 键值对RDD
## 常用的键值对RDD转换操作
+ reduceByKey: 归约后生成value-list并进行汇总求和
+ groupByKey: 归约后生成(key,value-list)，不会进行汇总求和
 ’ reduceByKey(_+_) ' 等价于' reduceByKey((a,b)=>a+b) ' ,'  groupByKey().map(t=>(t._1,t._2.sum)) '
+ keys :取出RDD中元素的键，并组成新的RDD
+ values: analog 类比keys方法的使用
+ sortByKey 与sortBy
+ mapValues :RDD元素的key不变，只对元素的value按传入的方法操作
+ join：将相同键的RDD进行连接
---------
# 文件数据读写
' val textFile=sc.textFile("directory") 读入'
' textFile.saveAsTextFile("writeback") 写出 '
## JSON文件的读写
' sc.textFile("xxx.json") '
利用scala中的JSON类的parseFull(strtext)进行json解析
## 读写HBase数据
+ HBase是Google 的bigtable的开源实现
+ HBase架构在HDFS基础上，只允许一次写入，多次读取，因此需借助时间戳来“修改文件”，
事实上旧版本的文件依然存在
+  关系型数据库采用二维定位数据，而HBase采用四维（* 行键、列族、列限定符、时间戳 *）定位数据
+  HBase为分布式数据库，利用集群和列族切分进行海量数据存储
### 创建一个HBase表
1. 创建列族信息，不必先创建表
如HBase-shell中的命令：
' hbase> disable 'student' '
' hbase> drop 'student' '
' hbase> create 'student','info' '  //student是表名，info是列族
2. 往单元格里添加数据
' put 'student','1','info:name' ,'Xueqian' '
' put 'student','1','info:age' ,'23' '

