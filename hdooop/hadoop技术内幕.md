# hadoop文件系统

Hadoop有一个抽象文件系统的概念，hdfs只是其中的一个实现，Java抽象类org.apache.hadoop.fs.FileSystem定义了hadoop中的一个文件系统接口，hdfs是实现了这个接口的一个文件系统，还有其它的文件系统实现，例如使用了本地磁盘文件系统的Local文件系统和RawLocalFilesystem等。

其他还有 

Hive   

Hbase   

S3等

#   生态

https://hadoopecosystemtable.github.io/