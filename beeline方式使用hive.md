# beeline方式的使用

## 使用mysql作为元数据存储方式

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?><configuration>

	<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://localhost:3306/metastore?createDatabaseIfNotExist=true</value>
		<description>JDBC connect string for a JDBC metastore</description>
	</property>

	<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
		<description>Driver class name for a JDBC metastore</description>
	</property>

	<property>
		<name>javax.jdo.option.ConnectionUserName</name>
		<value>root</value>
		<description>username to use against metastore database</description>
	</property>

	<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>123456</value>
		<description>password to use against metastore database</description>
	</property>
<property>
	<name>hive.server2.thrift.client.user</name>
	<value>root</value>
</property>
<property>
	<name>hive.server2.thrift.client.password</name>
	<value>123456</value>
</property>
</configuration>
```



## 使用之前先初始化元数据

```
schematool -dbType derby -initSchema
```

连接的时候,如果提示url不通,等一会重试

提示账号密码的问题去改hadoop的配置
```
<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
	</property>
	<property>
		<name>hadoop.proxyuser.server.hosts</name> 
		<value></value> 
	</property> 
	<property>
		<name>hadoop.proxyuser.server.groups</name>
		<value></value>
	</property>
	<property>
		<name>hadoop.proxyuser.root.hosts</name>
		<value></value>
	</property>
	<property>
		<name>hadoop.proxyuser.root.groups</name>
		<value></value>
	</property>
</configuration>
```

提示mysql驱动的问题,就去下载mysql的驱动包,然后放在hive的lib目录下

# SQL Developer的使用

## 先下载Hive JDBC Connector包

https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html

## 配置sql developer

添加hive的包即可

工具-首选项-数据库-第三方jdbc

将下载的jar全添加进去