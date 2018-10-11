Lua

# 注释

--注释

--[[注释]]

# 数据类型

## nil  无效值

​	作比较   a=="nil"

​	复制为nil相当于删除

## boolean 布尔值

## number 数值

## string 文本

```
用 2 个方括号 "[[]]" 来表示"一块"字符串。
在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:
以上代码中"error" + 1执行报错了，字符串连接使用的是 ..  如 print("a" .. 'b')
使用 # 来计算字符串的长度，放在字符串前面
```



## function 函数

```
function fn1(arg)

	...

end
```

## table	类似关联数组

```

local tbl = {"apple", "pear", "orange", "grape"}  //类似与索引数组，key为索引，值为value，索引从1开始

local tbl["name"]="张三"  --这是索引数组用法   tbl.name  是等价的
```

# 变量

只有local声明的才是局部变量，其他都是全局的，无论在哪，就算写在代码块里也是

复制

a=1 单赋值

a,b = 1,2  多变量赋值

a,b = fn()  用于函数返回两个值

# 循环

```
while(condition)
do
   statements
end
```

```
for var=exp1,exp2,exp3 do  
    <执行体>  
end  
```

```
for i, v in ipairs(a) do
    print(i, v)
end 
```
# 流程控制
```
if(布尔表达式)
then
   
end
```

```
if(布尔表达式)
then
   
else
   
end
```

```
if( 布尔表达式 1)
then 

elseif( 布尔表达式 2)
then
   
elseif( 布尔表达式 3)
then
   
else 
   
end
```