# 解决日志问题

```
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org").setLevel(Level.WARN)
```
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