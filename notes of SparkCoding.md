#RDD分区的作用和原则
##作用
1. 分区的第一个作用增加程序的并行度实现分布式的计算
2.  减少通信开销
##原则
分区个数=集群中**CPU核心数**
#设置分区的方法
1. 默认hashpartition 
2. 自定义分区方法Partitioner只支持元素类型为**键值对**的分区操作
*******
#键值对RDD
##常用的键值对RDD转换操作
+ reduceByKey: 归约后生成value-list并进行汇总求和
+ groupByKey: 归约后生成(key,value-list)，不会进行汇总求和
 ’reduceByKey(_+_)' 等价于'reduceByKey((a,b)=>a+b)' ,' groupByKey().map(t=>(t._1,t._2.sum))'
+ keys :取出RDD中元素的键，并组成新的RDD
+ values: analog 类比keys方法的使用
+ sortByKey 与sortBy
+mapValues :RDD元素的key不变，只对元素的value按传入的方法操作
+join：将相同键的RDD进行连接
---------
#文件数据读写
'val textFile=sc.textFile("directory") 读入'
‘textFile.saveAsTextFile("writeback") 写出’
##JSON文件的读写
‘sc.textFile("xxx.json")’
利用scala中的JSON类的parseFull(strtext)进行json解析

