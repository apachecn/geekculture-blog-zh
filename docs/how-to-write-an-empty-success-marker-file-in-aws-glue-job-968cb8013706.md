# 如何在 AWS 粘合作业中写入空的成功标记文件

> 原文：<https://medium.com/geekculture/how-to-write-an-empty-success-marker-file-in-aws-glue-job-968cb8013706?source=collection_archive---------12----------------------->

在这篇短文中，我将向你展示如何使用基于 Spark 的 AWS 粘合作业编写一个空的成功标记文件。

拥有一个空的成功标记文件是为了表明写操作已经成功完成。Spark 似乎默认启用了这个选项，但 AWS Glue 的 spark 不会在作业完成时自动写入一个`_SUCCESS`文件。

文档中没有谈到如何启用这个选项，不幸的是，很少有资源谈到这个选项。

首先，让我们定义一个`sparkSession`对象。

```
val spark: SparkContext = new SparkContext()
val glueContext: GlueContext = new GlueContext(spark)
val sparkSession: SparkSession = glueContext.getSparkSession
```

假设您有一个包含一些内容的数据框架`df`。

```
val data = Seq[Row](Row(1, "purple", "100$"), Row(2, "red", "50$"))
val df = sparkSession
.createDataFrame(sparkSession.sparkContext.parallelize(data))
```

接下来，如果需要将数据帧中的数据写入 S3 存储桶，可以使用以下方法:

```
df.write.mode("overwrite").csv("s3://<bucket_name>/data.csv")
```

上面的 write 语句不会生成成功标记，因为 AWS Glue job 没有默认启用的成功标记选项。

您可以使用下面的`option`来启用该选项。

```
df.write
.mode("overwrite")
.option("mapreduce.fileoutputcommitter.marksuccessfuljobs", true)
.csv("s3://<bucket_name>/<my_directory_path>/")
```

现在，一旦发出了`df.write`，一个空的`_SUCCESS`文件也将被写入目录。

注意:我们提供了一个 S3 目录路径，而不是指定文件名，因为 Spark 可能会根据输出文件的大小将文件分成多个部分。

你可以在官方文档中读到更多关于 AWS Glue 输出选项的内容。

 [## AWS Glue 中 ETL 输入和输出的格式选项

### “格式”和“格式选项”参数的可用设置。

docs.aws.amazon.com](https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-etl-format.html)