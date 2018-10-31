# 解决日志问题

```
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.WARN)
```
# spark sql的两种使用方式

1. 使用DSL

   就是使用Dataframe.groupBy   .sort  .where 这种的api函数

2. 使用sql风格

   就是使用sql语法,这个连接是spark内置的sql方言支持的函数列表:

   https://spark.apache.org/docs/latest/api/sql/index.html 

   spark.sql.dialect 配置spark使用的sql方言





# DataSet/DataFrame

## Actions

**显示数据的**

show	显示,truncate 输出值不会出现省略号

**取数据的**

first  第一个

head  前几个

take   前几个array形式

takeAsList  前几个list形式

collect	返回所有行集,Array形式

collectAsList	返回所有行集,List形式

toLocalIterator  返回一个迭代器

**计算的**

describe		基本的统计,min,max,mean,stddev...(全部计算)

summary	计算指定的统计,可选- count - mean - stddev - min - max - 百分比

count

foreach

foreachPartition



## base





cache  持久化,MEMORY_AND_DISK

checkpopint   返回一个checkpoint的DataSet,数据保存在SparkContext#setCheckpointDir目录

localCheckpoint

persist

storageLevel

unpersist



createGlobalTempView   跨session的,生命周期等同于应用程序的生命周期

createOrReplaceGlobalTempView

global_temp.view1   global_temp是系统创建的库,必须这样用



createTempView 只对当前session有效,局部的,生命周期等于创建dataset的session的周期

createOrReplaceTempView     

可以直接用   db.view  注意与全局的区别,那个必须用特定的库名



~~registerTempTable~~



as[U]   转换row的类型  这也是**DataFrame转为DataSet的方式**

toDF   转为DataFrame

javaRDD

toJavaRDD

rdd



columns 返回所有的列名

dtypes  返回所有列明和类型

printSchema

schema



write

writeStream

explain 打印执行计划

inputFiles

isLocal

## type transform

alias 别名

as 别名

coalesce  缩减分区,分区数目小于当前分区数缩减,大于,保持原分区数

distinct  删除重复行,针对行,也就是所有列

dropDuplicates  根据列名去重,针对的是某几个列

except 差集

intersect  交集

filter

flatMap



groupByKey

### 过滤操作

where

filter

### 查询操作

select

selectExpr	可以执行表达式，如：round(price)

col		能与select结合使用 如：df.select(df.col("name")).show()

apply	跟col一样

drop	去除指定字段



### 排序

orderBy

sort

sortWithinPartitions 分区内排序

```
df.select(df.col("name").desc).show()
df.select(-df.col("name")).show()  //效果一样，负号代表降序
```

### 分组，聚合

groupBy

agg	使用聚合函数df.groupBy(df.col("name")).agg("salary"->"max").show()

### 去重，limit

limit

distinct

### 关联

union

join



## DataFrame

相当于DataSet[row]



## Spark和hive的兼容性

spark并不是完全兼容hive的,有些udf是不能用的,但是hive的大部分语法,函数,特性,优化spark都可以用

不支持

- `getRequiredJars` and `getRequiredFiles` (`UDF` and `GenericUDF`) 自动包含udf所需的资源的函数
- `initialize(StructObjectInspector)` in `GenericUDTF` 仍没有支持. Spark SQL 当前仅仅使用了一个弃用的接口`initialize(ObjectInspector[])` 
- `configure` (`GenericUDF`, `GenericUDTF`, and `GenericUDAFEvaluator`) 初始化`MapredContext`的函数, 在spark中不适用.
- `close` (`GenericUDF` and `GenericUDAFEvaluator`) 释放资源的函数. 任务完成的时候,spark没有调用这个函数
- `reset` (`GenericUDAFEvaluator`) is a function to re-initialize aggregation for reusing the same aggregation. Spark SQL currently does not support the reuse of aggregation.
- `getWindowingEvaluator` (`GenericUDAFEvaluator`) is a function to optimize aggregation by evaluating an aggregate over a fixed window.

不兼容

- `SQRT(n)` If n < 0, Hive returns null, Spark SQL returns NaN.
- `ACOS(n)` If n < -1 or n > 1, Hive returns null, Spark SQL returns NaN.
- `ASIN(n)` If n < -1 or n > 1, Hive returns null, Spark SQL returns NaN.

# Rdd，DataFrame，DataSet区别

rdd，没有schema的约束，也就没有sql风格的用法，硬编码导致没有优化计划，如果是sql的话，可以在转成代码的时候优化一下

DataFrame 解决了rdd的痛点，有了优化，有了sql风格，但是sql没有检测（你字段名写错了呢？），注意此时的DataFrame虽然有schema，但是每行都是Row，所以只能通过字符串形式的sql



DataSet 综合以上有点，每行都是具体的类型了，sql可以使用强类型检查了



举个栗子：

a的值等于 b+1+1

rdd:   b+1  +1

dataframe ： b+2  （优化了）

个人感觉：

DataFrame相比rdd多了sql和优化，但是sql没有类型检查

dataset又比DataFrame的sql多了类型检查



# Structured streaming

## wordCount-基于sock

```
import org.apache.log4j.{Level, Logger}
import org.apache.spark.sql.SparkSession

object SimpleApp {
  def main(args: Array[String]) {

    val spark = SparkSession
      .builder
      .master("local[2]")
      .appName("StructuredNetworkWordCount")
      .getOrCreate()

    Logger.getLogger("org").setLevel(Level.WARN)

    import spark.implicits._

    val lines = spark.readStream
      .format("socket")
      .option("host", "192.168.140.181")
      .option("port", 9999)
      .load()

    // 将 lines 切分为 words
    val words = lines.as[String].flatMap(_.split(" "))

    // 生成正在运行的 word count
    val wordCounts = words.groupBy("value").count()

    // 开始运行将 running counts 打印到控制台的查询
    val query = wordCounts.writeStream
      .outputMode("complete")
      .format("console")
      .start()

    query.awaitTermination()

  }
}
```



# SparkSession

程序入口

```
SparkSession.builder
  .master("local")
  .appName("Word Count")
  .config("spark.some.config.option", "some-value")
  .getOrCreate()
```



# Hint

hint可以用来影响sql的执行方式,常用来告诉优化器使用怎么的优化







SparkSession

DataFrame

DataSet

Row

JavaRDD

RDD

Product

