# 独立\本地模式(单线程)
这种模式是以单线程模式启动,不用启动hdfs,yarn,直接运行就可以
```
$ mkdir input
$ cp etc/hadoop/*.xml input
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.1.jar grep input output 'dfs[a-z.]+'
$ cat output/*
```

# 伪分布式模式

配置免密登陆

```
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```
# 启动配置
core-site.xml
```
export HDFS_NAMENODE_USER="root"
export HDFS_DATANODE_USER="root"
export HDFS_SECONDARYNAMENODE_USER="root"
export YARN_RESOURCEMANAGER_USER="root"
export YARN_NODEMANAGER_USER="root"
```
hadoop.env
```
export JAVA_HOME=/usr/local/jdk1.8.0_181
```
# ERROR

java.lang.IllegalArgumentException: Invalid URI for NameNode address (check fs.defaultFS): file:/// has no authority.

```
这个是因为没有配置core-site.xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

# 重复格式化

删除hadoop.tmp.dir=/tmp/hadoop-${user.name}下的文件夹