# 解决日志问题

```
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.WARN)
```
# SparkSQL

## DataSet

### action类

show	显示

collect	返回一个Array

collectAsList	返回一个集合

describe		返回数值列的数值统计（最大 最小 平均 方差）

first	第一个

head	前几个，

take 前几个

> head在无参的时候返回T 有参返回Array   take无论何时都返回Array

takeAsList

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

## sql





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