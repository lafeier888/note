# 修复分区表

MSCK REPAIR TABLE table_name;

场景：有分区数据，表中元数据没有分区

可以自动添加表中元数据，也就是自动添加表分区



如果是有元数据，但是没有物理路径怎么解决？

删除表，重建表，修复分区表即可

# ToolBox介绍

使用的是OSGI+maven

项目中的组件都是osgi的插件，需要打包后放到plugins目录下，这个是配置的

# ToolBox环境搭建（Win）

## 需要导入的项目

assembly/
com.nsn.alarm/
com.nsn.cluster/
com.nsn.configurator/
com.nsn.datamining/
com.nsn.datamining.mysql/
com.nsn.datamining.postgres/
com.nsn.datamining.spark/
com.nsn.datamining.support.xdr.cmcc/
<u>*com.nsn.datamining.support.xdr.cmcc.cmdi/*</u>
<u>*com.nsn.datamining.support.xdr.cmcc.test/*</u>
com.nsn.do.base/
com.nsn.do.sqlloader.v2.oracle/
<u>com.nsn.do.tbox.cmcc.spark.test/</u>
*<u>com.nsn.do.tbox.cmcc.spark.volte/</u>*
*<u>com.nsn.do.tbox.cmcc.spark.volte.week/</u>*
com.nsn.do.tbox.mining/
com.nsn.executer/
com.nsn.framework/
com.nsn.io/
com.nsn.logger/
com.nsn.merger/
com.nsn.messages/
com.nsn.messages.client/
com.nsn.messages.driver/
com.nsn.scanner/
com.nsn.scheduler/
com.nsn.task/
com.nsn.util/
com.nsn.web/
com.nsn.web.configurator/
com.nsn.web.do/
com.nsn.web.do.nologin/
com.nsn.web.do.tbox/
<u>*com.nsn.web.do.tbox.cmcc.spark.test/*</u>
<u>*com.nsn.web.do.tbox.cmcc.spark.volte/*</u>
com.nsn.web.do.tbox.mining/
Framework/  --工具箱

斜体的项目是自己写的插件

## maven配置profile

``` xml
<profile>
  <id>nexus</id>
  <properties>
    <osgi_plugins_home>D:\project\Framework\plugins</osgi_plugins_home>
    <osgi_version>1.0-SNAPSHOT</osgi_version>
    <spark_lib_path>D:\project\com.nsn.datamining.spark\spark_lib</spark_lib_path>
    <common_lib_dir>D:\project\Framework\lib</common_lib_dir>
    <assembly_path>D:\project\assembly</assembly_path>
  </properties>
  <activation>
	<activeByDefault>true</activeByDefault>
  </activation>
</profile>
```
## 打包顺序

参考

assembly\install\Toobox\pom.xml

```xml
<modules>
		<module>../../../com.nsn.logger</module>
		<module>../../../com.nsn.configurator</module>
		<module>../../../com.nsn.util</module>
		<module>../../../com.nsn.io</module>
		<module>../../../com.nsn.executer</module>
		<module>../../../com.nsn.scheduler</module>
		<module>../../../com.nsn.merger</module>
		<module>../../../com.nsn.task</module>
		<module>../../../com.nsn.scanner</module>
		<module>../../../com.nsn.framework</module>
		<module>../../../com.nsn.alarm</module>
		<module>../../../com.nsn.cluster</module>
		<module>../../../com.nsn.datamining</module>
		<module>../../../com.nsn.datamining.spark</module>
		<module>../../../com.nsn.messages.client</module>
		<module>../../../com.nsn.messages.driver</module>
		<module>../../../com.nsn.datamining.auto</module>
		<module>../../../com.nsn.web</module>
		<module>../../../com.nsn.web.configurator</module>
		<module>../../../com.nsn.web.do</module>
		<module>../../../com.nsn.web.do.login</module>
		<module>../../../com.nsn.web.do.tbox</module>
	</modules>
```

## 启动

启动main类，启动之前需要配置环境变量FRAMEWORK_HOME=D:\dev\project\Framework

访问http://localhost:8080



win下可能需要删除flex-cache目录

work目录也有可能需要删除

# Framework项目介绍

这个就是工具箱启动项目,就是一个框架,工具箱的功能以组件的形式整合到框架中的plugins目录下

etc下是配置文件

​	this.properties

​	web.properties

​	这俩文件的端口保持一致

this.properties中的配置category=Toolbox-YUNNAN，可以通过知道工具箱读取的是哪个配置文件

plugins目录就是自己写的插件要放到哪（osgi的东西）

com.nsn.framework.Main这个是工具箱的启动类,里面配置了plugins,felix-cache,work三个目录，Main类需要环境变量FRAMEWORK_HOME



run.sh  启动工具箱

shutdown.sh 关闭工具箱

restart.sh 重启工具箱

console.log   日志文件

# 使用工具箱编写插件

需要编写三个东西:

1. 数据源
2. 业务逻辑处理
3. web

## 数据源

复制com.nsn.datamining.support.xdr.cmcc.cmdi

``` java
Activator中
/**
* 注册数据源
*/
 CmccXdrStreamSource.factory("test_factory", new test_factory());
```

factory包下:创建test_factory类,编写test.sql

```java 
String sql = IOUtils.toString(test_factory.class.getResourceAsStream("test.sql"), "UTF-8");
 return session.sql(sql);
// return session.sql(sql+" WHERE phour='"+start+"'");//测试的时候可能不需要分区,可以把这忽略了
```

注意对应关系:

Activator中注册的数据源类是test_factory类,将查询到的数据注册为一个临时视图test_factory

test_factory类绑定了test.sql文件 ,这儿sql文件负责查询数据

数据从哪来?  test_factory类的参数有一个sparkSession,spark整合了hive,所以就是从hive中取的数据



修改META-INF\MANIFEST.MF

```properties
Bundle-SymbolicName: com.nsn.datamining.support.xdr.cmcc.test
Bundle-Activator: com.nsn.datamining.support.xdr.cmcc.test.Activator
```

修改pom.xml

```xml
<project >
    <artifactId>com.nsn.datamining.support.xdr.cmcc.test</artifactId>
    <name>com.nsn.datamining.support.xdr.cmcc.test</name>
```

```xml
<build>
    <finalName>com.nsn.datamining.support.xdr.cmcc.test</finalName>
```

完事打包即可

## 业务逻辑

Activator.java

需要修改的是id,还有moudles

```java
private static final String ID = "test";

private static final String[][] modules = {
        {"test", "test专题", "/com/nsn/doo/tbox/cmcc/spark/test/test.xml"},
        {"test2", "test2专题", "/com/nsn/doo/tbox/cmcc/spark/test/test2.xml"},
        {"test3", "test3专题", "/com/nsn/doo/tbox/cmcc/spark/test/test3.xml"}

};
```

创建模块对应的xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<datamining id="sv" name="volte专题" type="spark1" driver="REQUIRED">
    <title>test专题</title>
    <description>test专题</description>
    <database id="spark1" type="XDR-CMCC-SPARK" name="HADOOP Env 1" runtime="true"/>
    <database id="hive" type="HIVE" name="hive结果库"/>
    <database id="hdfs" type="HDFS" name="hdfs结果"/>
    <window by="HOURLY"/>

    <!--<prepare type="class">com.nsn.datamining.support.xdr.cmcc.cmdi.broadcast.ConfigBroadcaster</prepare>-->
    <!--<prepare type="class">com.nsn.datamining.support.xdr.cmcc.cmdi.udf.UDFRegister</prepare>-->

    <source id="TEST_SOURCE" database="spark1" cache="false">
        <arg name="tables">test_factory</arg>
        <body><![CDATA[
         SELECT * FROM test_factory f
      ]]></body>
    </source>


    <summary id="F_VOLTE_SIP_SV_H_summary" in="TEST_SOURCE">
        <body><![CDATA[
            SELECT
            *
         from ${TEST_SOURCE}
        ]]></body>
    </summary>

    <result id="TEST_H" for="F_VOLTE_SIP_SV_H_summary" to="hdfs"/>


    <!--
        <post id="hdfsClean" type="cleaner">com.nsn.doo.tbox.cmcc.spark.moniluce3.post.CleanPost</post>
        <post id="oracleInvoker" type="cleaner">com.nsn.doo.tbox.cmcc.spark.moniluce3.post.InvokeOraclePost</post>
    -->
</datamining>
```

**database**:配置处理后的数据保存在哪里

**window**:专题执行粒度,比如选择了0-24点区间,时间粒度为小时,就会执行24次,每个小时一次

**source**:从数据源拿数据,然后注册为一个临时视图

​	ID:给summary用的

**summary**:具体业务逻辑,依赖的数据来自source或者其他的summary处理后的数据

​	id:标识,给result或者其他的summary用

​	in:依赖的视图,可以是source或者是其他summary的id

**result**:负责summary的结果和数据入库的对应关系

​	for:绑定summary的id

​	to:绑定database的id

​	id:如果是入hive,就是hive中的表名,如果是hdfs就是目录名

**prepare**:一些广播表和udf函数

**post**:

有个疑问,为什么不能省掉source,summary直接从数据源的临时视图或者是从其他的summary拿数据?

解答:  数据源可以多个,就有多个临时视图,而且数据源的数据参差不齐,有些我们都用不着

source这一层相当于只把我们需要的业务数据拿过来.

META-INF\MANIFEST.MF

```properties
Bundle-SymbolicName: com.nsn.do.tbox.cmcc.spark.test
Bundle-Activator: com.nsn.doo.tbox.cmcc.spark.test.Activator
```

pom.xml

```xml
<project>
    <artifactId>com.nsn.do.tbox.cmcc.spark.test</artifactId>
    <name>com.nsn.do.tbox.cmcc.spark.test</name>
```

```xml
<build>
    <finalName>com.nsn.do.tbox.cmcc.spark.test</finalName>
```

## web

Activator.java

```java
private static final String ID = "test";

private static final String[][] modules = {
        {"test", "test指标", "/com/nsn/web/doo/tbox/cmcc/spark/test/report.xml"},
        {"test2", "test指标2", "/com/nsn/web/doo/tbox/cmcc/spark/test/report.xml"},
        {"test3", "test指标3", "/com/nsn/web/doo/tbox/cmcc/spark/test/report.xml"}

};
```

META-INF\MANIFEST.MF

```properties
Bundle-SymbolicName: com.nsn.web.do.tbox.cmcc.spark.test
Bundle-Activator: com.nsn.web.doo.tbox.cmcc.spark.test.Activator
```

pom.xml

```xml
<project>
    <artifactId>com.nsn.web.do.tbox.cmcc.spark.test</artifactId>
    <name>com.nsn.web.do.tbox.cmcc.spark.test</name>
```

```xml
<build>
    <finalName>com.nsn.web.do.tbox.cmcc.spark.test</finalName>
```

# UI界面提交任务

--master yarn --deploy-mode client --driver-memory 12G --driver-cores 5 --executor-memory 8G --executor-cores 5 --num-executors 20 --conf spark.sql.shuffle.partitions=20 

# 消息机制

# 集群ToolBox配置

# 广播变量

# UDF

# post标签

# 源码



























com.nsn.web.do.tbox   web界面请求的入口

ExecuteServlet:POST 任务提交的入口



```
com.nsn.datamining.spark

spark1==>SparkDataminingEngine


DataminingEngine的注册
	注册DataminingEngine注册到com.nsn.datamining.DataminingEngine.Factory->engines中
	mysql ,MysqlDataminingEngine
	spark1,SparkDataminingEngine
	oracle,OracleDataminingEngine
	green,GreenDataminingEngine
	shell,ShellDataminingEngine
	memory,MemoryDataminingEngine

ExecuteServlet#doPost
	//这里获取到XmlDataminingFactory
	DataminingFactory df = DataminingFactory.getDataminingFactory(id);
	//实际调用的是XmlDataminingFactory
	DataminingFactory.requiredSources()  (com.nsn.datamining)
		XmlDataminingFactory.requiredSources()  (com.nsn.datamining)
			XmlDataminingFactory.init()  (com.nsn.datamining)
				DataminingEngine.load(InputStream, ClassLoader)  (com.nsn.datamining)
					XmlDatamining.load(InputStream)  (com.nsn.datamining.xml)
					DataminingEngine.load(XmlDatamining, ClassLoader)  (com.nsn.datamining)
	//
	DataminingEngine.xml()(2 usages)  (com.nsn.datamining)
	    MysqlDataminingEngine.xml()  (com.nsn.datamining.mysql.engine)
        MemoryDataminingEngine.xml()  (com.nsn.datamining.engines)
        OracleDataminingEngine.xml()  (com.nsn.datamining.engines)
        SparkDataminingEngine.xml()  (com.nsn.datamining.spark.engine)
        ShellDataminingEngine.xml()  (com.nsn.datamining.engines)
        GreenDataminingEngine.xml()  (com.nsn.datamining.engines)
	
每个专题启动后会调用DataminingFactory#register注册专题下每个模块对应的XmlDataminingFactory(id,xmlRes)等重要信息,每个模块注册后存放在DataminingFactory.factorys里


post方法执行后根据模块id在DataminingFactory.factorys查找对应的DataminingFactory(目前只有一个实现类,Xml的),然后调用它的XmlDataminingFactory#requiredSources方法.
XmlDataminingFactory#requiredSources首先执行XmlDataminingFactory.init()
XmlDataminingFactory.init()读取XmlDataminingFactory中的xml资源,转换为流对象
流对象传递给DataminingEngine.load(InputStream, ClassLoader)
DataminingEngine将流对象在传递给XmlXmlDatamining.load负责将流对象转换为XmlDatamining对象
拿到XmlDatamining对象之后,根据属性创建对应的DataminingEngine


requiredSources方法就是:初始化每个xml文件对应的DataminingEngine,并返回xml中定义的source

```

sbt 创建代码模板

understand 静态代码分析

vagrant

