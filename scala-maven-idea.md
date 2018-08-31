scala-maven-idea



## scala环境变量的配置

*前提配置好java

*配置SCALA_HOME

*错误: 找不到或无法加载主类 scala.tools.nsc.MainGenericRunner

	是因为scala目录含有空格

## IDEA+maven开发scala程序

如果用spark就加上spark-sql那个依赖,不用就只用scala的jar包即可

	*需要maven的scala插件

	*idea需要安装scala插件 

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lafeier.scala</groupId>
    <artifactId>demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>2.11.7</version>
        </dependency>
        <dependency> <!-- Spark dependency -->
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.11</artifactId>
            <version>2.3.1</version>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
            <groupId>net.alchim31.maven</groupId>
            <artifactId>scala-maven-plugin</artifactId>
            <executions>
                <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                </execution>
                <execution>
                    <id>scala-test-compile</id>
                    <phase>process-test-resources</phase>
                    <goals>
                        <goal>testCompile</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        </plugins>
    </build>
</project>
```

集合

可变集合:容量可以扩展

不可变集合:容量不可以扩展 

	scala中默认都是使用不可变集合

	不可变集合在可变集合中几乎都能找到对应版本(几乎,有的没有对应的)