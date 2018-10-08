Python

# *星号的使用

- ## 参数中出现

  *args  代表参数是元组

  ​	fn(*args)      fn(1,2,3)

  **args 代表参数是字典

  ​	fn(**args)  fn(name='lafeier',age=18)

- ## 函数调用的时候出现，表示解压参数

​        解压元组

  ​	a = (1,2,3)   

  ​	 fn(*a) = fn(1,2,3)

​        解压字典

  ​	a = {name:"lafeier",age:18} 

  ​	 fn(**a) = fn(name="lafeier",age=18)

# 函数式

lambda表达式实现函数定义

map

filter 

readuce

```
list = [1,2,3,4]
mapiter= map(lambda x:x+1,list)
for x in mapiter:
    print(x)
```

# 操作shell

commands模块只能用于linux

os.popen(command)  可用于windows

# http

requests模块

get(url,data,header)

​	data是参数 字典

​	header是协议头 字典


​	allow_redirects=False 禁止重定向


