# Structured Streaming Programming Guide

Structured Streaming是一种基于Spark SQL引擎的可扩展且容错的流处理引擎。您可以像表达静态数据的批处理计算一样表达流式计算。Spark SQL引擎将负责逐步和连续地运行它，并在流数据继续到达时更新最终结果。您可以使用Scala，Java，Python或R中的DataSet/DataFrameAPI来表示streaming aggregations, event-time windows, stream-to-batch joins等。计算在相同优化的Spark SQL引擎上执行。最终，系统通过检查点和预写日志确保端到端恰好一次的容错保证。简而言之，Structured Streaming提供快速，可扩展，容错，端到端的精确一次流处理，而无需用户理解straming。

在spark streaming内部默认使用微批处理引擎处理查询，该引擎将数据流作为一系列小批量作业处理，从而实现低至100毫秒的端到端延迟和**恰好一次**容错保证。

但是，自Spark 2.3以来，我们引入了一种称为 **Continuous Processing**的新型低延迟处理模式，它可以实现低至1毫秒的端到端延迟，且保证**至少处理一次**。您可以根据应用程序要求选择模式。在本指南中，我们将引导您完成编程模型和API，而无需更改查询中的Dataset/DataFrame操作。我们将主要使用默认的微批处理模型来解释这些概念，然后讨论**Continuous Processing**模型。首先，让我们从结构化流式查询的简单示例开始 - 流word count。

# 快速示例

假如你想保持运行一个word count程序，通过监听tcp sock的方式从一个数据服务器上获取数据。让我们看看如何使用Structured Streaming来表达这一点。您可以在Scala / Java / Python / R中看到完整代码。如果您下载了Spark，则可以直接运行该示例。无论哪种方式都行，让我们一步一步地了解示例并了解它是如何工作的。首先，我们必须导入必要的类并创建一个本地SparkSession，它是与Spark相关的所有功能的起点。

```scala
import org.apache.spark.sql.functions._
import org.apache.spark.sql.SparkSession

val spark = SparkSession
  .builder
  .appName("StructuredNetworkWordCount")
  .getOrCreate()
  
import spark.implicits._
```

接下来，我们创建一个 streaming DataFrame ，它表示从监听 localhost:9999 的服务器上接收的 text data （文本数据），并且将 DataFrame 转换以计算 word counts 。

```scala
// Create DataFrame representing the stream of input lines from connection to localhost:9999
val lines = spark.readStream
  .format("socket")
  .option("host", "localhost")
  .option("port", 9999)
  .load()

// Split the lines into words
val words = lines.as[String].flatMap(_.split(" "))

// Generate running word count
val wordCounts = words.groupBy("value").count()
```

此DataFrame类型的lines变量表示包含 streaming text data的无界表。此表包含一个string列，名为“value”，并且流式 streaming text data中的每一行都成为表中的一行（row）。请注意，现在我们并没有接收任何数据，因为我们只是设置了转换，还没有开始。