# com.nsn.datamining



```
WindowFactory接口

	static factorys  存放所有的窗口工厂类
	factorys()  返回所有的窗口工厂类
	Window list(start,end)  将start-end的时间段按照不同的窗口工厂类型切分,返回一组Window窗口
	name  返回
```



```
WindowFactory 实现类

	AllWindowFactory  就用一个Window,不切分

	MinutelyWindowFactory  按分钟切分时间段

		list方法:在start基础上,每次加1分钟,作为end  然后形成一个Window(start,end)

        小时,周,月,年同理
```



```
Window	窗口

	start

	end
```



```
StreamSourceRegistry
    sources  StreamSource
    types
```

```
ParameterInfo
	name   参数名
	title	
	description  描述
	type  参数的数据类型
```

```
StreamSource
	List<ParameterInfo> parameters();
	Properties properties(String prefix);
	void initialize(Map<String,Object> args)  初始化参数
	exists  表是否存在
```

```
Schemable
	SchemaInfo schema()
	Stream
```

```
SchemaInfo
```