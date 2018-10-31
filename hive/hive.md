# hive和hadoop的兼容性

![hive-hadoop兼容性](D:\note\assets\hive-hadoop兼容性.png)



# hive使用本地模式

set hive.exec.mode.local.auto=true;



# UDF

## 内置操作

.     			库.表   对象.属性

[] 			 数组[索引]

#### 算数操作

\+ -    正负

\+ - * / 

/和div的区别:  /的结果通常是双精度小数,除非配置[hive.compat](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-hive.compat) ,div的结果是整数

%取余

||    字符串连接

& | ^  按位  与，或，异或

\~ 按位not

#### 关系操作

is null/true/false

is not null/true/false

=  ==  相等判断

<=>  都Null返回True，都非NUll比较值返回true或false，其中一个是Null返回False

!=  <>   都为NULL为NUll ,  相等True,其他False

between

not between

A like   B  B是简单sql正则,只有_ %  _表示任意一个字符,%表示任意个字符

not like

A RLIKE   B  B就是一个正则表达式

A REGEXP  B

#### 逻辑操作

and  

or 

not

 !   等于not

in

not in

exists   看子查询是否返回至少一行

not exists

#### 字符串操作

||  拼接字符串

#### 类型操作

(k,v,k,v)  创建map

(v,v,v)  创建struct

(name,v,name,v) 创建name_struct

(v,v,v)数组

(tag,v,v,v) union类型



a[n]  数组操作

m[key]  map操作

s.x  struct操作




## 内置函数（常用）

### 数学函数

round/bround 舍入

floor/ceil	上下限

rand	随机

greatest  列表最大值

least	列表最后一个值



ln/log2/log10/log	对数

pow/power	幂

sqrt	开方

bin/hex/unhex/conv 进制转换

abs	绝对值

sin/asin/cos/acos/tan/atan 三角函数

degrees/radians 角度/弧度

### 集合函数

size

map_keys  返回所有key

map_values	返回所有value

array_contains	是否包含某个值

sort_array	排序数组

### 类型函数

cast(expr as <type>)  类型转换

### 时间函数

from_unixtime  从时间戳转为格式化字符串



unix_timestamp 当前时间戳/将字符串转为时间戳

current_timestamp 当前时间戳

current_date 当前时间(date对象)

add_months

to_date  时间戳转date对象



datediff/date_add/date_sub  时间差/时间增/时间减

时间戳转换

year/quarter/month/day/hour/minute/second

weekofyear

extract  从一个字符串中获取day, dayofweek, hour, minute, month, quarter, second, week and year.

==date_format==  时间格式转换

### 条件函数

if  isnull  isnotnull

nvl  null给一个默认值

COALESCE  返回第一个不为null的,都是null就返回null

case a    类似于switch吧

​	when b then c

​	when d then e

end



case  类似于if..elseif..elseif 

​	when a then b

​	when c then d

end

nullif(a,b)  a等b返回null,否则返回a

assert_true 如果条件为false 异常

### 字符串函数

ascii/chr   ascii和数值互转

character_length / length

concat

concat_ws

decode/encode

field  返回某个值在一系列值中的位置

find_in_set 返回一个字符串在一个逗号分隔的字符串列表中的位置

in_file/instr  看一个字符串是否出现在一个文件/字符串中

locate  查找子串出现的位置  类似indexOf

lower/upper

lpad /rpad将字符串使用指定的字符填充到指定长度,如果字符串的长度比指定的要长,会缩短字符串

ltrim/rtrim

parse_url

printf 格式化字符串

regexp_extract  正则提取,类似于组匹配

repeat 将一个字符串重复n次

reverse 翻转字符串

space 返回几个空格

split

str_to_map

substr  截取字符串

initcap  首字母大写

### mask字符串遮掩

15617840226   156\****0226

mask(str,upper,lower,number)  将大写变为X,小写变为x,数字变为n

mask_first_n  前几个

mask_last_n 后几个

mask_show_first_n

mask_show_last_n

mask_hash  字符出hash值

### 乱七八糟的函数

md5/sha/sha1/sha2/aes_decrypt/aes_encrypt

current_database

current_user

hash  不同于mask_hash  的是可以算多个字符串的hash值

java_method 通过反射调用java类

reflect

logged_in_user

version



### 统计和数据挖掘函数

**sentences**  将一个字符串拆为多个句子数组,每个句子拆为多个单词,最后算是个二位数组吧

这俩比较特殊,是词频统计用的



**context_ngrams**(array<array\<string>>, array\<string>, int K, int pf)

**ngrams**(array<array\<string>>, int N, int K, int pf)

第一个参数都是二维数组,一个句子数组,每个句子都是一个单词数组

N是几元组,K是返回的结果数

可以用来统计在所有句子里,二元组的出现频率

how old are you.how old he is.比如这句话

how old就是一个二元组,总共出现2次

how are

how you

are you

he is  这些都是出现了一次

context_ngrams的第二个参数可以用来指定需要的二元组,比如array('how','old')这样就是统计how old的组合在所有句子里出现的频次



**histogram_numeric**(col, b)  直方图  第一个参数是数据列,第二个 应该是要分的段数目吧



## 内置udaf函数（聚合函数）

count/sum/avg/min/max

collect_set  去重

collect_list 不去重

## 内置UDTF（表生成函数）

explode  用于array,map

inline 用于array(structt<>)

posexplode 带序号

stack 将n个值分为 r行  

select stack(2,a,b,c,d,e,f);	

a b c

d e f

parse_url_tuple  和parse_url一样,不过可以一次解析多个,返回的是多列



## 自定义UDF

**创建函数类,继承UDF类,重写evaluate方法**

```java
package com.example.hive.udf;
 
import org.apache.hadoop.hive.ql.exec.UDF;
import org.apache.hadoop.io.Text;
 
public final class Lower extends UDF {
  public Text evaluate(final Text s) {
    if (s == null) { return null; }
    return new Text(s.toString().toLowerCase());
  }
}
```
```
add jar my_jar.jar;  添加到类路径
list jars;
```

```sql
create temporary function my_lower as 'com.example.hive.udf.Lower';创建临时函数
create function my_db.my_lower as 'com.example.hive.udf.Lower';创建永久函数
CREATE FUNCTION myfunc AS 'myclass' USING JAR 'hdfs:///path/to/jar';  创建函数的时候指定

 select my_lower(title), sum(freq) from titles group by my_lower(title);
```

# Hive CLI

可以作为交互式或者非交互式模式使用

交互式模式就是直接 hive_home/hive直接进入

官方不推荐hive cli  推荐使用beeline

**非交互式模式使用方式**

hive -e "sql"  执行sql语句

hive -f xxx.sql  执行sql脚本



# 命令

**在hive cli和beeline中都可以中**

quit/exit   

set  输出所有用户自己重新配置的变量

set -v	输出所有hive和hadoop的配置变量

set <key>=<value>  重新配置变量

reset 重置所有属性为默认值



!<command>  执行终端命令

dfs <cmmand> 指定dfs命令

source FILE <filepath> 执行脚本



add file|archive|jar <path>

add files|archives|jars <paths*>



list file|archive|jar 

list files|archives|jars

