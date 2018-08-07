

# 安装nifi

修改配置文件conf/nifi.properties 

```
nifi.web.http.host=192.168.140.130
nifi.web.http.port=8080
```

启动/停止/状态

```
bin/nifi.sh start

bin/nifi.sh stop

bin/nifi.sh status
```

**UI**界面

http://192.168.140.130:8080

# NiFi组件

- FlowFile 
  `一个FlowFile代表每个被系统处理的数据对象`，一个FlowFile由两部分组成：属性（FlowFile Attributes）和内容（FlowFile Content）。内容是数据本身，属性是与数据相关的key-value的键值对，用于描述数据。所有flowFile都有以下标准属性：uuid、filename、path
- FlowFile Processor 
  Processor是NiFi的组件，`可以用来创建、发送、接受、转换、路由、割裂、合并、处理FlowFiles`。在用户建立数据流时，Processor是最重要的组成部分
- Connection 
  `提供Processors之间的连接，用来定义Processors之间的执行关系`，并允许不同Processors之间以不同的速度进行交互
- Flow Controller 
  其负责维护Processors之间的关联信息，并且管理所有进程对于线程的使用、分配
- Process Group 
  一个特定集合的Processors与它们之间的连接关系形成一个Process Group，其定义了从接受端口接受数据到通过发送端口发送数据之间，整个数据流的处理过程。并可以通过简单组合其它的部件来创建新的部件

远程debug

代码重新调试一次 